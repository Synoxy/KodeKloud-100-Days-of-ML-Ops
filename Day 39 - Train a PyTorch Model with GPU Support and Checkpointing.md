🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team is tasked with deploying fraud-detection models utilizing a compact PyTorch network. It is essential that the training script operates seamlessly on any available accelerator, whether that be CUDA GPUs in the production cluster or standard CPUs on the lab's nodes. This ensures uniform functionality across all platforms.

Currently, a preliminary version of the trainer is available at /root/code/fraud-detection/src/models/train_pytorch.py. However, this script is not device-aware; it assumes the presence of CUDA, resulting in failures upon the first tensor operation on incompatible hardware. Additionally, the device parameter logged to MLflow is hardcoded, leading to inaccuracies in reporting.

Your objective is to enhance the trainer by implementing device awareness, enabling it to accurately reflect the utilized device in the MLflow logs. Furthermore, you are required to incorporate per-epoch checkpointing to facilitate resuming long training sessions.

1. The MLflow tracking server is already running on port 5000. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty pytorch-training experiment. PyTorch (CPU build) is baked into the lab image; import torch works out of the box. The host does not expose a GPU (torch.cuda.is_available() returns False).

2. The project layout under /root/code/fraud-detection/:
   - data/train.csv – The same 200-row synthetic binary-classification dataset the rest of the Training section uses.
   - src/models/train_pytorch.py – The trainer scaffold. The two-layer feedforward network, the optimiser, the loss function, the MLflow experiment setup, and the model-persistence call to models/fraud_model.pt are already wired; the work is confined to this file (two device corrections plus a per-epoch checkpointing TODO in the training loop).
3. Run the trainer once against the scaffold as-is—python src/models/train_pytorch.py—to see it fail on the first tensor operation.
4. The end state must include:
   - The script completes successfully and writes a PyTorch state-dict to /root/code/fraud-detection/models/fraud_model.pt.
   - One run exists in the pytorch-training experiment on MLflow, carrying params.device = "cpu" and metrics.final_loss.
   - No bare .cuda() calls remain anywhere in train_pytorch.py.
   - At least two resumable checkpoints exist under /root/code/fraud-detection/checkpoints/ (named ckpt_epoch_*.pt), each a dict carrying the model and optimiser state (so training can resume).

🛠️ **Solution:**
1. Make below changes in ```train_pytorch.py```:
- Replace
model = FraudNet()
model = model.cuda()
with
```text
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = FraudNet().to(device)
```
- Replace
mlflow.log_param("device", "cuda")
with
```text
mlflow.log_param("device", str(device))
```
- Replace
xb = X_t.cuda()
yb = y_t.cuda()
with
```text
xb = X_t.to(device)
yb = y_t.to(device)
```
- Add below code after TODO
```text
if epoch % 10 == 0:
    ckpt_path = os.path.join(CHECKPOINT_DIR, f"ckpt_epoch_{epoch}.pt")
    torch.save({
        "epoch": epoch,
        "model_state_dict": model.state_dict(),
        "optimizer_state_dict": optimizer.state_dict(),
        "loss": final_loss,
    }, ckpt_path)
    print(f"Checkpoint saved: {ckpt_path}")
```
2. Run: ```python src/models/train_pytorch.py```. 