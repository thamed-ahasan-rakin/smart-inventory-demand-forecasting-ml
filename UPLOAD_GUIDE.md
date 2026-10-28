# GitHub upload steps

1. Create a new repository on GitHub named `smart-inventory-demand-forecasting-ml`.
2. Choose Public or Private. If you want to showcase this project on your portfolio, Public is typical; check with your teammates before publishing the paper.
3. Create the repository empty: do not select options to initialize it with a README, license, or `.gitignore`, because these are already included here.
4. Extract this ZIP, open a terminal in the extracted `smart-inventory-demand-forecasting-ml` folder, and run:

```bash
git init
git branch -M main
git add .
git status
git commit -m "Add smart inventory demand forecasting project"
git remote add origin https://github.com/thamed-ahasan-rakin/smart-inventory-demand-forecasting-ml.git
git push -u origin main
```

5. Before `git commit`, check `git status`. The CSV dataset and generated `.joblib` file should not appear because `.gitignore` excludes them.

If Git says the remote already exists, inspect `git remote -v` before changing it. If the GitHub repository already has commits (for example, a README initialized online), clone it first or reconcile the histories rather than blindly forcing a push.
