## Create a local kubernetes cluster

This vagrant script creates a new VM and installs a local kubernetes environment on it so you can run fabric8 in kube.

To try it out do:

    ./create.sh

This takes a while! Then you'll have a box kube1 which you can ssh into and run kube.


If you wish to use a separate Fedora image, then just run the **install-kube.sh** script as root on the box to install it all.
Note that after you have run the install script you need to log out and log back in again so that you have karma to run docker.

Then add these to your ~/.bashrc

    export GOPATH="/opt/go"
    export PATH=$GOPATH/bin:$PATH:/usr/bin:/opt/kube

### Running kube

Type the following to get into the kube installation:

    cd /opt/go/src/github.com/GoogleCloudPlatform/kubernetes

You can now boot up a kube via

    hack/local-up-cluster.sh

If it barfs saying

    Starting etcd
    ERROR: timed out for http://localhost:4001/v2/keys/

then edit the etcd startup line in hack/util.sh to:

    etcd -data-dir ${ETCD_DIR} -l ${host}:${port} >/tmp/etcd.log 2>/tmp/etcd.log &

### Running fabric8 in kube

Try this on the command line:

    curl https://raw.githubusercontent.com/fabric8io/fabric8-devops/master/vagrant/kubernetes/fabric8-master.json > fabric8-master.json
    kubecfg -c fabric8-master.json create pods