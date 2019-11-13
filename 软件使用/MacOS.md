### Homebrew

Homebrew 是MacOS的软件包管理器，它可以自动安装软件的依赖包，非常便捷。

- /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
- brew doctor 确认正常工作
- brew update 升级
- brew install mysql 安装MySQL
- brew cask install docker 安装 docker

#### 官方镜像源
1. brew.git
- cd "$(brew --repo)" 
- git remote set-url origin https://github.com/Homebrew/brew.git
2. homebrew-core.git
-  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
-  git remote set-url origin https://github.com/Homebrew/homebrew-core.git
3. homebrew-bottles
- vi ~/.bash_profile
- 删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
- source ~/.bash_profile

#### 中科大镜像源
1. brew.git
- cd "$(brew --repo)"
- git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
2. homebrew-core.git
- cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
- git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
3. homebrew-bottles
- **对于bash用户:**
- echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
- source ~/.bash_profile
- **对于zsh用户**
- echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
- source ~/.zshrc 