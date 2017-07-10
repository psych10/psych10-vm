# this was adapted from nipype Vagrantfile

VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT



# install nipype dependencies
$HOME/miniconda2/bin/conda update --yes conda
$HOME/miniconda2/bin/pip install setuptools
$HOME/miniconda2/bin/conda install --yes pip numpy scipy nose traits networkx
$HOME/miniconda2/bin/conda install --yes dateutil ipython-notebook matplotlib
$HOME/miniconda2/bin/conda install --yes statsmodels boto  pandas scikit-learn
$HOME/miniconda2/bin/pip install flask
$HOME/miniconda2/bin/pip install gunicorn
$HOME/miniconda2/bin/pip install Flask-AutoIndex




sudo apt-get update > /dev/null
sudo apt-get install -y --force-yes libssl-dev
sudo apt-get install -y --force-yes r-base
sudo apt-get install -y --force-yes git
sudo apt-get install -y --force-yes r-base-dev
sudo apt-get install -y --force-yes xserver-xorg-core
sudo apt-get install -y --force-yes lightdm
sudo apt-get install -y --force-yes lxde
sudo apt-get install -y --force-yes chromium-browser
sudo apt-get install -y --force-yes virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11
sudo sed -i 's/allowed_users=.*$/allowed_users=anybody/' /etc/X11/Xwrapper.config


# install latest ruby using rbenv
if [ ! -d ~/.rbenv ]
then
   git clone https://github.com/rbenv/rbenv.git ~/.rbenv
   echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   echo 'eval "$(rbenv init -)"' >> ~/.bashrc

   git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
   echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
   ~/.rbenv/bin/rbenv install 2.4.1
   ~/.rbenv/bin/rbenv global 2.4.1
   ruby -v
   gem install bundler
fi

#install node.js
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install nodejs

sudo apt-get -y --force-yes install gdebi-core
wget https://download1.rstudio.org/rstudio-1.0.143-amd64.deb
sudo gdebi -n rstudio-1.0.143-amd64.deb
rm rstudio-1.0.143-amd64.deb

sudo sh -c 'echo "[SeatDefaults]
user-session=LXDE
autologin-user=vagrant
autologin-user-timeout=0" >> /etc/lightdm/lightdm.conf'

# NGINX install
sudo apt-get install -y --force-yes nginx

if [ ! -d $HOME/R_libs ]
then
mkdir $HOME/R_libs
echo "export R_LIBS_USER=$HOME/R_libs" >> .bashrc
echo "export R_LIBS_USER=$HOME/R_libs" >> .env
fi

#echo '[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && exec startx' >> ~/.bashrc

# NGINX SETUP
#sudo /etc/init.d/nginx start
#sudo rm /etc/nginx/sites-enabled/default

sudo VBoxClient --clipboard
sudo VBoxClient --draganddrop
sudo VBoxClient --display
sudo VBoxClient --checkhostversion
sudo VBoxClient --seamless

SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.forward_x11 = true

  config.vm.define :engine do |engine_config|
    engine_config.vm.box = "ubuntu/trusty64"

    engine_config.vm.network :private_network, ip: "192.128.0.20"
    engine_config.vm.hostname = 'psych10'
    #engine_config.vm.synced_folder "/tmp/myconnectome", "/home/vagrant/myconnectome", create: true

    engine_config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--memory", "4096"]
      vb.customize ["modifyvm", :id, "--cpus", "2"]
      vb.gui = true
    end

    engine_config.vm.provision "shell", :privileged => false, inline: $script
  end
end
