🚨 **Problem:**
The xFusionCorp Industries ML team is adopting DVC so that datasets and model files are versioned separately from code. Initialise DVC inside the existing Git repository at /root/code/fraud-detection/ and record the initialisation in Git.

- A Git repository already exists at /root/code/fraud-detection/ with an initial commit.

- Initialise DVC inside that repository so that the standard .dvc/ control directory and .dvcignore file are created alongside the existing Git working tree.

- Stage every file that DVC produces during initialisation, and record them in a new Git commit with the message Initialize DVC.

Once initialisation is complete, the DVC extension will detect the new .dvc/ directory and surface the DVC TRACKED section in the EXPLORER panel together with a DVC indicator in the bottom status bar.

🛠️ **Solution:**
1. Navigate to project folder and run git status.
2. Run **dvc init** and you should see something like: 
3. Run git status and now you should see 3 new files under **.dvc**
4. Run **git add .** followed by **git commit -m "Initialize DVC"**
5. Run **git log** to check the commits made and you should see **Initialize DVC** commit.