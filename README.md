# haproxy Loadbalancer

## Create Cloud-init 
    cat << EOF | tee script.sh > /dev/null 2>&1
    #cloud-config
    debug: True
    runcmd:
     - sudo apt-get update
     - sudo add-apt-repository ppa:vbernat/haproxy-1.7
     - sudo apt install -y haproxy
     - sudo mv /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.orig
     - wget https://raw.githubusercontent.com/teghitsugaya/loadbalancer/main/haproxy.cfg
     - mv haproxy.cfg /etc/haproxy/haproxy.cfg
     - sudo systemctl restart haproxy
    EOF

## Create Server with User-data
    openstack server create --flavor GP.1C2G --image "Ubuntu 22.04 LTS" --network Public_Subnet02_DC2 --security-group allow-all --availability-zone AZ_Public01_DC2 --key-name teguh-4-hp --user-data script.sh "Loadbalancer"
