🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team is adopting Feast as the feature store for their fraud-detection workflow. The first steps are to scaffold a working feature repository with the Feast CLI, apply the starter definitions to the local registry, build a point-in-time training set from the offline store, and confirm everything loads in the Feast UI. Your task is to initialise a feature repository under /root/code/, apply the registry, complete the pre-staged build_training_set.py so it generates a training set via get_historical_features, and verify the project in the Feast UI.

1. Feast is already installed in the lab image and the feast CLI is on PATH.

2. The target project layout:
   - /root/code/feature_repo/feature_repo/feature_store.yaml – The feast init scaffold config (provider, registry, online/offline stores).
   - /root/code/feature_repo/feature_repo/data/registry.db – Written by feast apply from the repo root.
   - /root/code/feature_repo/feature_repo/feature_definitions.py – The starter feature definitions Feast ships with the scaffold (a driver_hourly_stats feature view over data/driver_stats.parquet).
   - /root/code/build_training_set.py – Pre-staged. Reads (driver_id, event_timestamp) rows from the source and is meant to build a training set via get_historical_features; the retrieval call is left as a # TODO.

3. The end state must include:
   - The /root/code/feature_repo/feature_repo/ directory is populated with the feast init scaffold.
   - feature_store.yaml parses as valid YAML and carries the project, provider, and registry keys.
   - data/registry.db exists – feast apply completed without error.
   - build_training_set.py calls store.get_historical_features(entity_df=…, features=["driver_hourly_stats:conv_rate", "driver_hourly_stats:acc_rate", "driver_hourly_stats:avg_daily_trips"]).to_df(), and running it writes /root/code/training_set.parquet carrying those joined feature columns (a point-in-time training set).
   - The Feast UI button at the top of the lab opens a responsive dashboard that lists the scaffold's project.

feast ui is a long-running process; run it in a second VS Code terminal (or append & to the command) so the shell remains usable. The UI loads the registry at start-up—start the UI after feast apply has written registry.db.

🛠️ **Solution:**
1. Run ```feast init feature_repo``` in terminal.
2. Navigate to feature_repo folder and run ```feast apply```
3. Add below code block after TODO in ```build_training_set.py```.
```text
training_df = store.get_historical_features(
    entity_df=entity_df,
    features=[
        "driver_hourly_stats:conv_rate",
        "driver_hourly_stats:acc_rate",
        "driver_hourly_stats:avg_daily_trips",
    ],
).to_df()
```
4. Run ```python build_training_set``` in terminal after Navigating back to code repo.
