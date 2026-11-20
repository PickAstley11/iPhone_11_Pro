## Introduction
This is a repo of which the purpose is to automate shsh blob saving. This repo saves Baseband, AP and cryptex1. 

## How it works
I have a cronjob at https://cron-job.org which starts the workflow every 30 minutes. Once the workflow runs, it will look into a private repo and see which shsh blobs have already been saved there. Then it will query Apple´s TSS (Tatsu Signing Server) server for blobs for every ipsw that has ever existed for the iPhone 11 Pro. If TSS refuses, the workflows continues. If TSS signs the ipsw the workflow will save a blob unless if it is already archived (in that case it wont query TSS at all. It will then push these blobs into my pivate repo so that they will get archived. All sensitive values like APNONCE, BBSNUM (baseband serial number), BLOBS_REPO_TOKEN (the github access token used to push the blobs into my private repo), CRYPTEX_NONCE, CRYPTEX_SEED, DISCORD_WEBHOOK(used for messaging me in my server when a ipsw gets unsigned and when a new blob gets saved), ECID and the GENERATOR are all saved as github secrets. This workflow would most likely also catch older firmwares being accidently resigned by Apple. If you would like to fork this and make your own implementation please go ahead, the workflow usees the arm64 tsschecker release (I recommend using the latest release) to save blobs and check the signing status. 

## How to make this your own:
1.Fork this repo

2.At /.github/worklow/auto_saver.yml edit line 74 and replace iPhone12,3 with the model identifier of your device and then save the changes.

3.Click the Settings button under the repo, then go click on "Secrets & Variables" and then from the dropdown menu choose actions

4.Make new repository secrets with the names excluding the " named "APNONCE", "BBSNUM", "BLOBS_REPO_TOKEN", "CRYPTEX_NONCE", "CRYPTEX_SEED", "DISCORD_WEBHOOK", "ECID" and "Generator". These have to be dumped from your own device.
For the device specific identifiers like the cryptex seed and APNONCE, you need a jailbroken device and x8A4 more info here:
https://github.com/1Conan/tsschecker/blob/main/README.md
For the Discord webhook, go to your desired discord server click on the server name, choose Server Settings from the dropdown menu go to Integrations, choose webhook and make a new webhook and paste in the webhook url into the secret. 
For the BLOBS_REPO_TOKEN first make a new repository mine is named iPhone11_Pro_Blobs and make sure its private (If not everyone will be able to see your shsh blobs and device secrets which we tried to keep secret via our github secrets.
Now click on your github profile picture, click Settings, scroll all the way down to developer settings, click on Personal Access Tokens and select Fine-Grained token from the dropdown menu. 
I would name it BLOBS_REPO_TOKEN, have it select for only a few repos and select your blob saving repo. When selecting the permissions select Contents and change it to read/write.

5.Now that we have the entire workflow set up, its time to test it so go back to your fork of this repo and go to the actions tab, select agree to use your workflows and run the workflow Auto SHSH Blob Saver.
Afterwards I recommend making an account at https://console.cron-job.org/ and making a account, make a new Cronjob and fill out the information like this:
Title: Blobsaver
URL: https://api.github.com/repos/PickAstley11/iPhone_11_Pro/actions/workflows/auto_saver.yml/dispatches
(replace PickAstley11 with your username
Execution Scedule: I have it set to 30 minutes but you can make it longer if you want to
Now go to the advanced tab and make 2 new headers.
Key:Value
User-Agent:CronJob
Authorization:Bearer YOUR_CLASSIC_ACCESS_TOKEN (go back to settings-> developer settings -> Personal Access tokens (classic) and make a new one with repo ticked
To see if it works before saving it you can click test run and you should see a github workflow start.

6.Profit!!!!??!!!
