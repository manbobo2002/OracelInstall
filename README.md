# Oracel DB (12c) Install  

# get root right  
su root  
#password  

# For someone who did not install git  
sudo yum install git-all -y  

# start  
yum install -y binutils.x86_64 compat-libcap1.x86_64 gcc.x86_64 gcc-c++.x86_64 glibc.i686 glibc.x86_64 \
glibc-devel.i686 glibc-devel.x86_64 ksh compat-libstdc++-33 libaio.i686 libaio.x86_64 libaio-devel.i686 libaio-devel.x86_64 \
libgcc.i686 libgcc.x86_64 libstdc++.i686 libstdc++.x86_64 libstdc++-devel.i686 libstdc++-devel.x86_64 libXi.i686 libXi.x86_64 \
libXtst.i686 libXtst.x86_64 make.x86_64 sysstat.x86_64
groupadd oinstall  
groupadd dba  
useradd -g oinstall -G dba oracle  
passwd oracle  
#TYPE THE PASSWORD  

git clone https://github.com/manbobo2002/Oracle-Install.git  
rm -rf /etc/sysctl.conf  
mv /home/$USER/Oracle-Install/sysctl.conf /etc/sysctl.conf  
rm -rf /etc/security/limits.conf  
mv /home/$USER/Oracle-Install/limits.conf /etc/security/limits.conf  
yum groupinstall -y "X Window System"  


# Download Oracle DB manually and change to oracle account  

# get root right  
su root  
#password  

yum -y install zip unzip  

# Extract the Oracle files to a new directory named 'stage'  
unzip linuxx64_12201_database.zip -d /stage/  

chown -R oracle:oinstall /stage/  
mkdir -p /u01 /u02  

chown -R oracle:oinstall /u01 /u02  
chmod -R 775 /u01 /u02  
chmod g+s /u01 /u02  

# new terminal  

# get root right  
su root  
#password  

# Install Oracle
ip=$(ip route get 8.8.8.8 | awk -F"src " 'NR==1{split($2,a," ");print a[1]}')  
ssh -X oracle@$ip  
#TYPE THE PASSWORD  
cd /stage/database/  
./runInstaller  

# Oracle setting
/*
Oracle base: '/u01/app/oracle'  
Software location: /u01/app/oracle/product/12.1.0/dbhome_1  
Database file location: /u02  
Database edition: Default  
Character set: Default  
OSDBA group: dba  
Global database name: Type your own name  
Administrative password: Type your own password  
Confirm password: Type again  
Uncheck the 'Create as Container database'  

At 'Create Inventory', enter the path below:  

Inventory Directory: /u01/app/oraInventory  

oraInventory Group Name: use 'oinstall' group.  
*/

# After installation
ssh root@ip  
/u01/app/oraInventory/orainstRoot.sh  
/u01/app/oracle/product/12.2.0/dbhome_1/root.sh  

# Testing
sqlplus / as sysdba  
alter user sys identified by yourpassword;
