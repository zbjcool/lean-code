npm在node中
安装node就有npm

直接用yum install nodejs
安装的版本很老

第1步：添加NodeSource存储库

Node.js和NPM的最新版本可从官方的NodeSource Enterprise Linux，Fedora，Debian和Ubuntu二进制发行版存储库获得，该存储库由Nodejs网站维护，您需要将其添加到您的系统才能安装最新的Nodejs和NPM包。



---------- 安装 Node.js v11.x ---------- 
$ curl -sL https://rpm.nodesource.com/setup_11.x | bash -

---------- 安装 Node.js v10.x ----------
$ curl -sL https://rpm.nodesource.com/setup_10.x | bash -

sudo yum install -y nodejs


