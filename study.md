# uv tool install nanobot-ai
让 uv 去下载、安装别人发布的 nanobot-ai 这个工具，找不到 → 直接报错，完全不会自动去读你本地的 pyproject.toml/ 源码
uv tool install . 才会安装你本地的包，需要在pyproject.toml同目录
读取包里自带的 pyproject.toml
看到 [project.scripts] 那一行，自动给你创建 nanobot 命令
给系统装一个全局命令 nanobot， 并且自动创建一个完全隔离的虚拟环境，不污染你的系统 Python。
uv tool install 会把命令放到 ~/.local/bin
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# [pyproject.toml](pyproject.toml)
[project.scripts]
nanobot = "nanobot.cli.commands:app" 
当你在终端输入 nanobot 这个命令时，Python 会去执行 nanobot.cli.commands 模块里的 app 对象。
这是 Python console script（控制台脚本） 的标准写法，
对应你项目里的文件：nanobot/cli/commands.py

按顺序它到底做了哪 6 件事：
1. uv 检查 nanobot-ai 在 PyPI 上是否存在
2. 自动创建一个独立、干净的虚拟环境
3. 在隔离环境里安装 nanobot-ai 及其依赖
4. 自动把 nanobot 命令软链接到系统 PATH
5. 自动处理版本、依赖、冲突
6. 安装完成后输出：成功添加命令

# nanobot 初始化
nanobot onboard
[nanobot.cli.commands:app]
nanobot          →  文件：nanobot/cli/commands.py
nanobot onboard  →  文件里的 @app.command() 函数：onboard()

# nanobot/cli/commands.py
import typer
app = typer.Typer()  # 入口

# 👇 这一行，对应终端命令：nanobot onboard
@app.command()
def onboard():
    """这里就是 onboard 命令真正执行的代码"""
    print("开始执行 onboard 逻辑...")



