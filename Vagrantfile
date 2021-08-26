require 'yaml'

$SETUP = <<-SHELL
sudo su
echo -e "change_me\nchange_me" | passwd root
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
sed -in 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
yum install net-tools wget -y
timedatectl set-timezone America/Sao_Paulo
systemctl restart sshd chronyd

if [ $(hostname) == "uf1.local.com" ] || [ $(hostname) == "uf2.local.com" ]
then
cd /opt &&  wget -O splunkforwarder-8.0.3-linux-2.6-x86_64.rpm 'https://www.splunk.com/bin/splunk/DownloadActivityServlet?architecture=x86_64&platform=linux&version=8.0.3&product=universalforwarder&filename=splunkforwarder-8.0.3-a6754d8441bf-linux-2.6-x86_64.rpm&wget=true'
cd /opt && yum install -y splunkforwarder-8.0.3-linux-2.6-x86_64.rpm && /opt/splunkforwarder/bin/splunk start --accept-license --answer-yes --no-prompt --seed-passwd change_me
/opt/splunkforwarder/bin/splunk enable boot-start
/opt/splunkforwarder/bin/splunk set deploy-poll 192.168.50.13:8089 -auth admin:change_me
/opt/splunkforwarder/bin/splunk add forward-server 192.168.50.11:9997 -auth admin:change_me
/opt/splunkforwarder/bin/splunk add forward-server 192.168.50.12:9997 -auth admin:change_me
/opt/splunkforwarder/bin/splunk add monitor /var/log/ -auth admin:change_me
/opt/splunkforwarder/bin/splunk restart
else
cd /opt && wget -O splunk-8.2.0-e053ef3c985f-linux-2.6-x86_64.rpm 'https://www.splunk.com/bin/splunk/DownloadActivityServlet?architecture=x86_64&platform=linux&version=8.2.0&product=splunk&filename=splunk-8.2.0-e053ef3c985f-linux-2.6-x86_64.rpm&wget=true'
yum install -y splunk-8.2.0-e053ef3c985f-linux-2.6-x86_64.rpm && /opt/splunk/bin/splunk start --accept-license --answer-yes --no-prompt --seed-passwd change_me
/opt/splunk/bin/splunk enable listen 9997 -auth admin:change_me
/opt/splunk/bin/splunk enable boot-start
/opt/splunk/bin/splunk restart
fi
SHELL

yaml = YAML.load_file("machines.yml")

Vagrant.configure("2") do |config|
  yaml.each do |server|

    config.vm.define server["name"] do |srv|
      srv.vm.box = server["system"]
      srv.vm.network "private_network", ip: server["ip"]
      srv.vm.hostname = server["hostname"]
      srv.vm.provision "shell", privileged: true,inline: $SETUP
      srv.vm.provider "virtualbox" do |vb|
      srv.vm.disk :disk, size: "30GB", primary: true
        vb.name = server["name"]
        vb.memory = server["memory"]
        vb.cpus = server["cpus"]
      end
    end
  end
end