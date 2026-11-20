## Introduction
This is a repo of which the purpose is to automate shsh blob saving. This repo saves Baseband, AP and cryptex1. 

## How it works
I have a cronjob at https://cron-job.org which starts the workflow every 30 minutes. Once the workflow runs, it will look into a private repo and see which shsh blobs have already been saved there. Then it will query Apple´s TSS (Tatsu Signing Server) server for blobs for every ipsw that has ever existed for the iPhone 11 Pro. If TSS refuses, the workflows continues. If TSS signs the ipsw the workflow will save a blob unless if it is already archived (in that case it wont query TSS at all. It will then push these blobs into my pivate repo so that they will get archived. All sensitive values like APNONCE, BBSNUM (baseband serial number), BLOBS_REPO_TOKEN (the github access token used to push the blobs into my private repo), CRYPTEX_NONCE, CRYPTEX_SEED, DISCORD_WEBHOOK(used for messaging me in my server when a ipsw gets unsigned and when a new blob gets saved), ECID and the GENERATOR are all saved as github secrets. This workflow would most likely also catch older firmwares being accidently resigned by Apple. If you would like to fork this and make your own implementation please go ahead, the workflow usees the arm64 tsschecker release (I recommend using the latest release) to save blobs and check the signing status. 

## How to make this your own:
For this repo: 
