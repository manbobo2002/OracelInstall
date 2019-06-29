# OracelInstall

#get root right
su root
#password

# For someone who did not install git
sudo yum install git-all
y

#start
rm /etc/sysctl.conf
y
git clone https://github.com/manbobo2002/OracelInstall/blob/master/sysctl.conf
mv /home/$USER/OracleInstall/sysctl.conf /etc/sysctl.conf
