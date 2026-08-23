# Deploying to Streamlit Community Cloud

Order matters: the organisation must exist **before** you connect Streamlit, or
the OAuth prompt will not offer organisation access and you will have to
disconnect and start again.

## 1. Create the repository

In the `celnicconsulting` organisation, create a **public** repository named
`msd-social-support-explorer`. Do not initialise it with a README — this build
already has one.

Then, from this folder:

```bash
git init
git branch -M main
git add .
git commit -m "MSD Social Support Explorer: public build"
git remote add origin https://github.com/celnicconsulting/msd-social-support-explorer.git
git push -u origin main
```

The data file is 20 MB, under GitHub's 50 MB warning threshold, so Git LFS is not
needed.

## 2. Deploy

1. Go to https://share.streamlit.io and sign in with GitHub
2. At the OAuth prompt, grant **organisation access** to `celnicconsulting`
3. Click **Create app**, then choose the existing repository
4. Set:
   - Repository: `celnicconsulting/msd-social-support-explorer`
   - Branch: `main`
   - Main file path: `app/msd_social_support_explorer.py`
   - **Custom subdomain**: `celnic-msd` (gives `celnic-msd.streamlit.app`)
5. Deploy

First build takes a few minutes while dependencies install.

## 3. After deploying

- Put the live URL in `README.md` where it says _add your Streamlit URL here_
- Add the org profile: create a **public** repository named `.github` in the
  organisation and commit `profile/README.md` from `public_repo_org_profile/`.
  GitHub renders it on https://github.com/celnicconsulting

## Resource envelope

Community Cloud allows up to 2.7 GB memory, 2 CPU cores and 50 GB storage. This
app loads a 20 MB DuckDB file read-only and caches query results, so it sits well
inside those limits.

## Updating the data later

Rebuild the extract and push:

```bash
python scripts/13_build_public_mart.py
cp public/msd_platform_public.duckdb <repo>/data/
git commit -am "Refresh data extract" && git push
```

Community Cloud redeploys automatically on push to the tracked branch.
