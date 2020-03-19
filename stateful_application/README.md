Stateful app intended to be used with stateful application

When you have an app which requires persistence, you should create a stateful set instead of deployment.

There are many benefits. Also, you will not have to create PVCs in advance, and you will be able to scale it easily.

With the stateful set, you can define a volumeClaimTemplates so that a new PVC is created for each replica automatically
