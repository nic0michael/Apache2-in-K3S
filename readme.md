
# Deploying Apache2 Web-server in the kubernetes Cluster
After noticing that this sublect is not treated well in YouTube we decided to do a proper job.\
Here is our Source Code:

## 1. Get the basic Apache project working in your kubernetes Cluster
please start here: \
[Instruction to do this](https://github.com/nic0michael/Apache2-in-K3S/tree/master/original-working-project)

## 2. Adding PV and PVC
We will now add a **Persistent Volume PV and a Persistent Volume Claim PVC** \
This is required to have a permanent storage of the Web Server Content
[Instruction to do this](https://github.com/nic0michael/Apache2-in-K3S/tree/master/using-pv-and-pvc)

## 3. Enableing HTTPS
We will now **create a Self-signed certificate** and add this to the project \
[Instruction to do this](https://github.com/nic0michael/Apache2-in-K3S/tree/master/apache-withSelf-signed-cert)
