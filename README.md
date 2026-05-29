data/
├── train/
│   ├── dose_sys_train.csv       # training target
│   ├── covariates.csv           # training covariates
│   └── images/mat_density/      # MAT density PNG sidecar
│       └── {ST}_{PERIOD_ID}.png
├── val/
│   ├── covariates.csv           # validation covariates (target hidden)
│   └── images/mat_density/      # MAT density PNG sidecar (val periods)
│       └── {ST}_{PERIOD_ID}.png
├── sample_submission.csv        # submission template — fill this and write to ../submission.csv
└── Data_Description.md          # this file
