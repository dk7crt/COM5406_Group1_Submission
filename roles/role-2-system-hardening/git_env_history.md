# Git History Forensics – .env file

**Command:** `git log -p --all -- .env`

**Output (key lines):**

commit 592874a70a5196f9184cca37c269262ae6cb65c3
Author: maureen_bpboss <maureen@beepiesandpasties.co.uk>
Date:   Thu Nov 6 01:58:43 2025 +0000
    add env config

diff --git a/.env b/.env
new file mode 100644
index 0000000..a73aca1
--- /dev/null
+++ b/.env
@@ -0,0 +1,5 @@
+SECRET_KEY=bpbpies_secret_key_2024
+DB_PASSWORD=freddibnah1938
+AWS_ACCESS_KEY_ID=AKIAXM7V3BEEPIEZ2025
+AWS_SECRET_ACCESS_KEY=mAUReen/bpbpiES/S3Cr3t/KEY/Fr3d1938
+S3_BUCKET=bee-pies-pasties-backups

commit 9ec58127c1848b4448fcf5c77636f58488e350a1
Author: maureen_bpboss <maureen@beepiesandpasties.co.uk>
Date:   Thu Nov 6 02:18:47 2025 +0000
    remove env config

diff --git a/.env b/.env
deleted file mode 100644
index a73aca1..0000000
--- a/.env
+++ /dev/null
@@ -1,5 +0,0 @@
-SECRET_KEY=bpbpies_secret_key_2024
-DB_PASSWORD=freddibnah1938
-AWS_ACCESS_KEY_ID=AKIAXM7V3BEEPIEZ2025
-AWS_SECRET_ACCESS_KEY=mAUReen/bpbpiES/S3Cr3t/KEY/Fr3d1938
-S3_BUCKET=bee-pies-pasties-backups

## Significance
- The `.env` file was added and then deleted by Maureen, but remains in Git history.
- The AWS secret key contains `mAUReen` – a direct personal reference linking the credential to Maureen.
- An attacker (or investigator) can recover these secrets from Git history even after they are deleted from the working tree.