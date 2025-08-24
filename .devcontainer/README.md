# Chain Mini DevContainer

这是一个为 Cosmos SDK 开发优化的开发容器配置，基于 `chain-minimal` 项目。

## 功能特性

### 🚀 开发环境

- **Go 1.23** - 最新稳定版本
- **自定义镜像** - 基于官方 Go 镜像构建
- **完整工具链** - 预装所有必要的开发工具

### 🛠️ 预装工具

- `golangci-lint` - Go 代码质量检查
- `goimports` - 自动导入管理
- `gopls` - Go 语言服务器
- `cobra-cli` - CLI 应用生成器
- `protoc-gen-gocosmos` - Cosmos protobuf 生成器
- `protoc-gen-go` - 标准 protobuf 生成器
- `protoc-gen-go-grpc` - gRPC 生成器
- `goreleaser` - Go 发布工具

### 📡 Cosmos 端口

- **26656** - P2P 端口
- **26657** - RPC 端口
- **1317** - API 端口
- **9090** - gRPC 端口

## 快速开始

### 1. 安装 VS Code 扩展

确保安装了 [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) 扩展。

### 2. 打开项目

```bash
git clone https://github.com/cosmosregistry/chain-minimal.git
cd chain-minimal
code .
```

### 3. 启动开发容器

当 VS Code 检测到 `.devcontainer` 目录时，会提示你：

- 点击右下角的 **"Reopen in Container"** 按钮
- 或者在命令面板中执行：`Dev Containers: Reopen in Container`

### 4. 等待初始化

容器启动后会自动执行以下操作：

- 构建自定义 Docker 镜像
- 安装 Go 模块依赖
- 配置开发工具
- 设置开发环境

### 5. 验证环境

容器启动完成后，在终端中验证：

```bash
# 检查 Go 版本
go version

# 检查预装工具
golangci-lint --version
goimports --version
gopls version

# 检查项目依赖
go mod tidy
go mod verify
```

## 开发工作流

### 构建项目

```bash
# 安装依赖
go mod tidy

# 构建二进制文件
make install

# 运行测试
go test ./...
```

### 启动区块链

```bash
# 初始化网络
make init

# 启动节点
minid start
```

### 代码质量检查

```bash
# 运行 linter
golangci-lint run

# 格式化代码
goimports -w .

# 检查代码
go vet ./...
```

## 常用命令

### Cosmos SDK 命令

```bash
# 查看帮助
minid --help

# 创建账户
minid keys add my-account

# 查看账户
minid keys show my-account

# 查询余额
minid query bank balances <address>

# 执行转账
minid tx bank send <from> <to> <amount> --chain-id test
```

### 开发工具命令

```bash
# 生成 protobuf 代码
make proto-gen

# 运行模拟
make sim

# 检查依赖
go mod verify

# 更新依赖
go get -u ./...
```

## 配置说明

### 环境变量

- `GOROOT=/usr/local/go` - Go 安装路径
- `GOPATH=/home/vscode/go` - Go 工作空间
- `PATH` - 包含所有必要的二进制文件路径

### VS Code 设置

- 启用了 Go 语言服务器
- 配置了代码格式化工具
- 设置了文件关联（YAML, Protobuf）
- 优化了 Go 开发体验

## 故障排除

### 常见问题

#### 1. 容器启动失败

```bash
# 清理 Docker 资源
docker system prune -a

# 重新构建容器
Dev Containers: Rebuild Container
```

#### 2. Go 工具未找到

```bash
# 重新安装工具
go install golang.org/x/tools/gopls@latest
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

#### 3. 权限问题

```bash
# 检查用户权限
whoami
groups

# 修复权限
sudo chown -R vscode:vscode /workspace
```

### 日志查看

```bash
# 查看容器日志
docker logs <container-id>

# 查看应用日志
minid start --log_level debug
```

## 自定义配置

### 添加新的 Go 工具

在 `Dockerfile` 中添加：

```dockerfile
RUN go install github.com/your-tool/cmd@latest
```

### 修改端口映射

在 `devcontainer.json` 中更新：

```json
"forwardPorts": [26656, 26657, 1317, 9090, 8080]
```

### 更新 Go 版本

在 `Dockerfile` 中修改：

```dockerfile
ARG VARIANT="1-1.24-bookworm"  # 改为新版本
```

## 最佳实践

### 1. 代码组织

- 使用模块化设计
- 遵循 Go 代码规范
- 编写完整的测试

### 2. 依赖管理

- 定期更新依赖
- 使用 Go modules
- 锁定版本号

### 3. 开发流程

- 使用 Git 分支开发
- 编写清晰的提交信息
- 进行代码审查

## 学习资源

- [Cosmos SDK 文档](https://docs.cosmos.network/)
- [Go 官方文档](https://golang.org/doc/)
- [Dev Containers 文档](https://code.visualstudio.com/docs/remote/containers)

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个开发容器配置！

---

**注意**: 这个项目已经被归档，建议使用 [Ignite CLI](https://github.com/ignite/cli) 来构建最小化区块链：

```bash
ignite s chain mini --minimal
```
