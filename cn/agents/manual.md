# 手动配置 Agent

任务是运行在 Agent 上的，所以在任务开始之前，需要创建并启动至少一个 Agent。

> 如果从 [docker-install](https://github.com/FlowCI/docker-install.git) 安装 flow.ci, 默认会配置一个自动 Agent 启动器，所以可以不用配置 Agent，直接运行任务即可

## 1. 从管理员页面创建 Agent

* 点击 `Settings` -> `Agents` -> `+`
* 选择 `Manual agent`
* 输入一个名称
* 定义标签 (可选)

  Agent 标签用于 YAML `selector` 配置，可以让工作流运行在指定的 Agent 中。例如在 Agent 中配置了 `ios` 标签，并且在 YAML 中定义了如下的 `selector`， 则该工作流只会运行在带有 `ios` 标签的 Agent 中.

  ```yaml
  selector:
    label:
      - ios
  ```

* 保存 `Save`
  
  新创建的 Agent 会在列表中显示，可以从列表中拷贝 token

![how to create agent](../../src/agents/create_agent.gif)

## 2. 启动 Agent

### 使用 Agent Image 启动

最简单的方式为 Git 克隆 [docker-install](https://github.com/flowci/docker-install) 仓库，之后运行 `./agent.sh -t <token> -u <server url> start`，填入 token 和 server url 即可。

或者可以使用以下脚本，填入 `FLOWCI_SERVER_URL` 和 `FLOWCI_AGENT_TOKEN`

```bash
docker volume create pyenv
docker run --rm -v pyenv:/ws flowci/pyenv:1.0 bash -c "~/init-pyenv-volume.sh"

docker run -it \
-e FLOWCI_SERVER_URL=<http://yourhost:port> \
-e FLOWCI_AGENT_TOKEN=<token_copied_from_admin_page> \
-e FLOWCI_AGENT_VOLUMES="name=pyenv,dest=/ci/python,script=init.sh" \
-v /var/run/docker.sock:/var/run/docker.sock \
flowci/agent
```

### 可执行程序 (binary) 启动

```bash
# wget https://github.com/FlowCI/flow-agent-x/releases/download/<version>/flow-agent-x-<os>

wget https://github.com/FlowCI/flow-agent-x/releases/download/v0.20.30/flow-agent-x-linux
chmod +x flow-agent-x-linux
./flow-agent-x-linux -u <ci_server_url> -t <agent_token>

# ex: ./flow-agent-x-linux -u http://172.10.20.1:8080 -t 44793491-03ac-4a3c-8c59-1f09b7c9d0e3
```
