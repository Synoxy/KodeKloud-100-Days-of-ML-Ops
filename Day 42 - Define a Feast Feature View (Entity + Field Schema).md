🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team keeps the fraud-detection feature definitions in a Feast repository at /root/code/fraud-detection/feature_repo/. A draft features.py exists there: the source, the entity, and the feature-view shell are scaffolded, but the entity's join key and the served feature schema are unfinished. Your task is to author the entity join key and the feature schema to match the source in data/transactions.parquet, apply the registry, and confirm the customer_transaction_features view in the Feast UI.

1. The Feast UI is already running on port 8888. The Feast UI button at the top of the lab can be opened to confirm—the dashboard loads the fraud_detection project with one entity and one feature view carrying the draft declarations.

2. The repository layout under /root/code/fraud-detection/feature_repo/:
   - feature_store.yaml – The Feast config (project fraud_detection, local provider, sqlite online store, file offline store). Correct and must remain intact.
   - data/transactions.parquet – A 200-row synthetic source keyed by customer_id; carries amount as Float32, hour + num_tx_past_day + is_fraud as Int64, and an event_timestamp column. Correct and must remain intact.
   - features.py – Declares the FileSource, the customer Entity (join key unfinished — uses a placeholder that is not in the source), and the customer_transaction_features FeatureView (schema unfinished — only amount is declared, at a placeholder type). Author the join key and the full served schema here.
   - data/registry.db – Written by feast apply at startup from the draft; must be re-applied after the edits so the registry reflects the authored declarations.

3. The end state must include:
   - The customer entity in the registry has join_keys = ["customer_id"].
   - The customer_transaction_features view declares all three served features with source-matching dtypes: amount Float32, hour Int64, num_tx_past_day Int64 (the is_fraud label is not served).
   - feast apply exits without error and the Feast UI reflects the authored entity and feature-view schema.

The Feast UI's Entities and Feature Views tabs surface the applied values directly—the current (draft) values are visible there so the required change is easy to eyeball against the task's end-state.

🛠️ **Solution:**
1. Update the code for customer entity in the registry with ```join_keys=["customer_id"]```.
2. Update the schema in TODO 2:
```text
schema=[
        Field(name="amount", dtype=Float32),
        Field(name="hour",dtype=Int64),
        Field(name="num_tx_past_day",dtype=Int64)
    ]
```
3. Navigate to feature_repo in fraud_detection module and run ```feast apply```