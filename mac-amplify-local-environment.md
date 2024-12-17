# Mac版_Amplify Gen2開発ローカル環境構築

## 目次
- [Mac版\_Amplify Gen2開発ローカル環境構築](#mac版_amplify-gen2開発ローカル環境構築)
  - [目次](#目次)
  - [事前にひとりでやれる範囲](#事前にひとりでやれる範囲)
    - [**1. Xcode Command Line Toolsのインストール**](#1-xcode-command-line-toolsのインストール)
    - [**2. Homebrewのインストール**](#2-homebrewのインストール)
    - [**3. Visual Studio Code (VS Code) のインストール**](#3-visual-studio-code-vs-code-のインストール)
    - [**4. Gitのインストール**](#4-gitのインストール)
    - [**5. NVM (Node Version Manager) のインストール**](#5-nvm-node-version-manager-のインストール)
    - [**6. Node.jsのインストール**](#6-nodejsのインストール)
    - [**7. AWS CLI v2のインストール**](#7-aws-cli-v2のインストール)
  - [分かる人といっしょにやる範囲](#分かる人といっしょにやる範囲)
    - [**8. GitとGitHubの接続（アクセストークンを使用）**](#8-gitとgithubの接続アクセストークンを使用)
      - [(1) Gitの初期設定](#1-gitの初期設定)
      - [(2) Personal Access Tokenの生成](#2-personal-access-tokenの生成)
      - [(3) トークンでHTTPS接続](#3-トークンでhttps接続)
    - [9. **Mac環境でposh-gitライクな環境の整備**](#9-mac環境でposh-gitライクな環境の整備)
      - [**(1) bash-completionのインストール**](#1-bash-completionのインストール)
      - [**(2) posh-gitの代替：`posh-git-sh`のセットアップ**](#2-posh-gitの代替posh-git-shのセットアップ)
        - [**(2)-1 `posh-git-sh`リポジトリをクローン**](#2-1-posh-git-shリポジトリをクローン)
        - [**(2)-2 git-prompt.shのコピー**](#2-2-git-promptshのコピー)
      - [**(3) .bash\_profileの更新**](#3-bash_profileの更新)
      - [**(4) zsh環境向けの設定**](#4-zsh環境向けの設定)
      - [**表示例**](#表示例)


## 事前にひとりでやれる範囲
ひとりでやっても効率的な作業
### **1. Xcode Command Line Toolsのインストール**

```bash
xcode-select --install
```

コマンドラインツールをインストールします。インストール確認のダイアログが表示されたら「インストール」を選択してください。

---

### **2. Homebrewのインストール**

Homebrewはパッケージマネージャで、ツールのインストールを簡単にしてくれます。

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

インストール完了後、以下で確認：

```bash
brew --version
```

---

### **3. Visual Studio Code (VS Code) のインストール**

```bash
brew install --cask visual-studio-code
```

インストール後、VS CodeをCLIから起動するコマンドを有効化：

```bash
cat <<EOF >> ~/.zshrc
# VS Code CLI
export PATH="\$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
EOF
source ~/.zshrc
```

確認：

```bash
code --version
```

---

### **4. Gitのインストール**

Homebrewを使って最新バージョンをインストールします。

```bash
brew install git
```

確認：

```bash
git --version
```

---

### **5. NVM (Node Version Manager) のインストール**

NVMはNode.jsのバージョン管理ツールです。

```bash
brew install nvm
```

NVMを有効化するため、以下を`.zshrc`に追記：

```bash
cat <<EOF >> ~/.zshrc
# NVM
export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion"
EOF
source ~/.zshrc
```

確認：

```bash
nvm --version
```

---

### **6. Node.jsのインストール**

nodeのインストールしたいバージョンをインストールします。（下の例ではAmplify Gen2環境の標準である`18.17`をインストールする例）

```bash
nvm install 18.17.0
nvm use --lts
```

確認：

```bash
node --version
npm --version
```

---
### **7. AWS CLI v2のインストール**
ブラウザで以下のpkgファイルをダウンロードし、インストールを実行してください。
- [Install on Mac](https://awscli.amazonaws.com/AWSCLIV2.pkg)

## 分かる人といっしょにやる範囲
分かる人とやったほうが効率的な範囲
### **8. GitとGitHubの接続（アクセストークンを使用）**

#### (1) Gitの初期設定
- `Your Name`のところには、Githubのユーザ名を入力
- `your_email@example.com`のところには、Githubの登録メールアドレスを入力

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

#### (2) Personal Access Tokenの生成

1. GitHubで「Developer Settings」→「Personal Access Tokens」→「Tokens (classic)」から新しいトークンを生成。
2. 必要なスコープ（`repo`, `read:org`など）を選択。

#### (3) トークンでHTTPS接続

認証情報を保存：

```bash
git config --global credential.helper osxkeychain
```

最初のリモート接続時に、`password`入力においてPersonal Access Tokenを入力すれば認証が通る  
例: 
```
git clone https:/https://github.com/some-user/some-repository
# 初回実行すると、認証でユーザ名とパスワードが求められる
# ユーザ名にGithubユーザ名、パスワードにPersonal Access Tokenを入力
```

### 9. **Mac環境でposh-gitライクな環境の整備**

#### **(1) bash-completionのインストール**

`bash-completion`がインストールされていない場合は、Homebrewを使用してインストールします。

```bash
brew install bash-completion
```

インストール後の指示に従い、`~/.bash_profile`に以下を追加します：

```bash
[[ -r "/opt/homebrew/etc/profile.d/bash_completion.sh" ]] && . "/opt/homebrew/etc/profile.d/bash_completion.sh"
```

設定を反映：

```bash
source ~/.bash_profile
```

---

#### **(2) posh-gitの代替：`posh-git-sh`のセットアップ**

**posh-git-sh**は、`posh-git`のBash版で、Mac環境でGit情報をシェルプロンプトに表示します。

##### **(2)-1 `posh-git-sh`リポジトリをクローン**

```bash
git clone https://github.com/lyze/posh-git-sh.git ~/posh-git-sh
```

##### **(2)-2 git-prompt.shのコピー**

`git-prompt.sh`スクリプトをローカルに配置します。

```bash
cp ~/posh-git-sh/git-prompt.sh ~/
```

---

#### **(3) .bash_profileの更新**

`~/.bash_profile`を更新して、`git-prompt.sh`を読み込み、プロンプトを設定します。

1. `~/.bash_profile`をVS Codeで開く：
    
    ```bash
    code ~/.bash_profile
    ```
    
2. 以下の設定を追加：
    
    ```bash
    # bash-completionの読み込み
    [[ -r "/opt/homebrew/etc/profile.d/bash_completion.sh" ]] && . "/opt/homebrew/etc/profile.d/bash_completion.sh"
    
    # git-prompt.shの読み込み
    source ~/git-prompt.sh
    
    # posh-git-shのプロンプト設定
    PROMPT_COMMAND='__posh_git_ps1 "\u@\h:\w" "\\\$ ";'$PROMPT_COMMAND
    ```
    
3. 保存して、設定を反映：
    
    ```bash
    source ~/.bash_profile
    ```
    

---

#### **(4) zsh環境向けの設定**

macOS Catalina以降、**zsh**がデフォルトシェルになっています。`bash`ではなく`zsh`を使っている場合、以下の設定が必要です。

1. `~/.zshrc`を開く：
    
    ```bash
    code ~/.zshrc
    ```
    
2. 以下を追加：
    
    ```bash
    # bash-completion互換
    [[ -r "/opt/homebrew/etc/profile.d/bash_completion.sh" ]] && . "/opt/homebrew/etc/profile.d/bash_completion.sh"
    
    # git-prompt.shの読み込み
    source ~/git-prompt.sh
    
    # PROMPTの設定 (zsh用)
    setopt PROMPT_SUBST
    precmd() { __posh_git_ps1 "%n@%m:%~" "\$ " }
    ```
    
3. 設定を反映：
    
    ```bash
    source ~/.zshrc
    ```
    

---


#### **表示例**

- **≡**: ローカルブランチがリモートと同期している。
- **+**: ステージ済みのファイルがある。
- **~**: 変更済みのファイルがある。
- **⚡**: コンフリクトが発生している。

