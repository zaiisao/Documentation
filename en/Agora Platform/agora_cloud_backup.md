
---
title: Agora Cloud Backup 
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:47:16 GMT+0800 (CST)
---
# Agora Cloud Backup 
Agora Cloud Backup is a backup cloud storage service used in Cloud Recording. If the recording service cannot upload the recorded files to the specified third-party cloud storage, then the service automatically and temporarily stores them in the backup cloud. If the uploading resumes, the recording service transfers the recorded files to the third-party cloud storage and deletes them from the backup cloud. 

The backup cloud stores recorded files for a maximum of 10 days, and then automatically deletes them. Developers can discover whether the recorded files are uploaded to the backup cloud via an Agora Cloud Recording RESTful API callback.

<div class="alert info">See also: <a href="https://docs.agora.io/en/cloud-recording/cloud_recording_callback_rest?platform=All%20Platforms">Agora Cloud Recording RESTful API Callback Service</a></div>

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
