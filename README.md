# Oracel DB (11g) Install  

# get root right  
su root  
#password  

# For someone who did not install git  
sudo yum install git-all  
y  

# start  
groupadd oinstall  
groupadd dba  
useradd -g oinstall -G dba oracle  
passwd oracle  
#TYPE THE PASSWORD  

git clone https://github.com/manbobo2002/Oracle-Install.git  
rm /etc/sysctl.conf  
y  
mv /home/$USER/Oracle-Install/sysctl.conf /etc/sysctl.conf  
rm /etc/security/limits.conf  
y  
mv /home/$USER/Oracle-Install/limits.conf /etc/security/limits.conf  
yum groupinstall -y "X Window System"  


# Download Oracle DB manually and change to oracle account  

# get root right  
su root  
#password  

yum -y install zip unzip  

# Extract the Oracle files to a new directory named 'stage'  
unzip linux.x64_11gR2_database_1of2.zip -d /stage/  
unzip linux.x64_11gR2_database_2of2.zip -d /stage/  

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
