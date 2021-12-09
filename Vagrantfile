Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network :forwarded_port, guest: 5432, host: 5433

  config.vm.provider "virtualbox" do |vb|
  vb.gui = false
  vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
  
  sudo wget https://ftp.postgresql.org/pub/source/v8.4.0/postgresql-8.4.0.tar.bz2
  sudo tar xvjf postgresql-8.4.0.tar.bz2
  sudo apt-get update
  sudo apt-get install -y build-essential libreadline-dev zlib1g-dev flex bison libxml2-dev libxslt-dev libssl-dev clang gcc
  cd postgresql-8.4.0/
  ./configure CFLAGS="-O1" CC=clang
  make
  sudo make install
  sudo useradd -p postgres postgres
  sudo mkdir /usr/local/pgsql/data
  sudo chown postgres /usr/local/pgsql/data
  su -c "/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data" postgres
  sudo mkdir /usr/local/pgsql/data/log
  sudo chown postgres /usr/local/pgsql/data/log
  su -c '/usr/local/pgsql/bin/pg_ctl start -l /usr/local/pgsql/data/log/logfile -D /usr/local/pgsql/data' postgres
  echo "su -c '/usr/bin/pg_ctl start -l /usr/local/pgsql/data/log/logfile -D /usr/local/pgsql/data' postgres" >> /etc/rc.local

  SHELL
end
