# Challenge 8: Storage

[< Previous Challenge](./07-updaterollback.md) - **[Home](../README.md)** - [Next Challenge >](09-networking.md)

## Introduction

What happens when your application needs to have some persistent storage? 

## Description

In this challenge we will be creating Azure data disks and using the Kubernetes storage mechanism to make them available to be attached to running containers. This will give MongoDB a place to store its data without losing it when the container is stopped and restarted.

- Verify what happens without persistent storage
	- Make sure that you are using the v2 versions of the Fabmedical container images
	- Delete the MongoDB pod created in Challenge 6. 
		- Kubernetes should automatically create a new pod to replace it. 
		- Connect to the new pod; you should observe that the “contentdb” database is no longer there since the pod was not configured with persistent storage.
	- Delete your existing MongoDB statefulset.
- Redeploy MongoDB with dynamic persistent storage
	- Create two Persistent Volume Claims (PVC) using the new Storage Class, one for data and one for config.
    	- Look in the Resources/Challenge 8 folder for starter templates
	- Modify your MongoDB statefulset set to use the PVCs.
    	- Again, look in the `Resources/Challenge 8` folder for starter templates
	- Deploy MongoDB
		- Examine the automatically provisioned Persistent Volumes (PVs) and verify that both are in the same zone.
		- Check that the disk and the node that the pod runs on are in the same zone
- Verify that persistent storage works
	- Verify that MongoDB is working fine by connecting to the corresponding MongoDB Pod in interactive mode. Make sure that the disks are associated correctly (bold and italic below)

		- `kubectl exec -it <mongo-db pod name> bash`
		- `root@mongo-db678745655b-f82vj:/#` **`df -Th`**
			<pre><code>	Filesystem     Type     Size  Used Avail Use% Mounted on
			overlay        overlay   30G  4.2G   25G  15% /
			tmpfs          tmpfs    1.7G     0  1.7G   0% /dev
			tmpfs          tmpfs    1.7G     0  1.7G   0% /sys/fs/cgroup
			<b><i>/dev/sdc       ext4     2.0G  304M  1.5G  17% /data/db
			/dev/sdd       ext4     2.0G  3.0M  1.8G   1% /data/configdb</i></b>
			/dev/sda1      ext4      30G  4.2G   25G  15% /etc/hosts
			shm            tmpfs     64M     0   64M   0% /dev/shm
			tmpfs          tmpfs    1.7G   12K  1.7G   1% /run/secrets/kubernetes.io/serviceaccount
			tmpfs          tmpfs    1.7G     0  1.7G   0% /sys/firmware</code></pre>

		- `root@mongo-db678745655b-f82vj:/#` **`mongo --version`**
			```
			MongoDB shell version v3.6.1
			connecting to: mongodb://127.0.0.1:27017
			MongoDB server version: 3.6.1
			```

	- Re-load the sample content (Speakers & Sessions data) in to MongoDB by running the content-init job as you did earlier during Challenge 7.
	- Make sure that the `contentdb` database is populated by connecting to the MongoDB pod with an interactive terminal and verify the database "contentdb" exists.
		- `root@mongo-db678745655b-f82vj:/#` **`mongo`**
			```
			MongoDB shell version v3.6.1
			connecting to: mongodb://127.0.0.1:27017
			MongoDB server version: 3.6.1
			
			> show dbs
			admin       0.000GB
			config      0.000GB
			contentdb   0.000GB
			local       0.000GB
			```

	- Destroy the MongoDB pod to prove that the data persisting to the disk 
		- `kubectl delete pod <mongo-db-pod>`, or more elegantly, `kubectl rollout restart statefulset mongodb`
	- Wait for kubernetes to recreate the pod
	- Once the pod is created, verify that data is persisted by following the previous MongoDB verification step.
	- Verify the API can retrieve the data by viewing the speaker and sessions pages in the browser: 
		- `http://<yourWebServicePIP>:3000/speakers.html`
		- `http://<yourWebServicePIP>:3000/sessions.html`

## Success Criteria

1. Speaker and session data is imported into Mongo
1. Stop and restart the MongoDB pod and make sure the data wasn't lost.
1. Verify that this new MongoDB instance and its data are used in the application.


## Extra Credit
Discuss with your coach what would happen if the Node running the MongoDB pod were to crash or be shut down.  Would Kubernetes be able to re-deploy the pod on a different node?  How do availability zones play into this?
