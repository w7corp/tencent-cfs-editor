# cfs 代码编辑


## 简介

laravel-starter 模板使用 Tencent SCF 组件及其触发器能力，方便的在腾讯云创建，配置和管理一个 laravel-starter 应用。

## 快速开始

### 1. 安装

```bash
# 安装 Serverless Framework

npm install -g serverless
```

### 2. 创建

通过如下命令直接下载该例子：

```bash
git clone https://gitee.com/free-worker/tencent-cfs-editor.git
cd tencent-cfs-editor
```
#### 2.1 修改serverless.yml
> 修改如下标记　自定义处
``` yaml
component: scf
name: tencentcfseditora
inputs:
  name: tencentcfseditora
  region: ap-shanghai
  namespace: default
  # 指定 SCF 类型为 Web 类型
  type: web
  memorySize: 128 # 
  timeout: 20 # 
  initTimeout: 15 # 
  environment: #  
    variables: # 
      VSCODE_EDITOR_ROOT: /mnt/  # 不能用/mnt 必须 /mnt/    自定义
      VSCODE_SDK_API_HTTPS: true   #文件请求走https协议
  vpcConfig: # 私有网络配置
    vpcId: vpc-8ezuc5hk # 私有网络的Id   自定义
    subnetId: subnet-7yu9qe5v # 子网ID   自定义
  cfs: # cfs配置
    - cfsId: cfs-eaplmrx7 #自定义
      mountInsId: cfs-eaplmrx7 #自定义
      localMountDir: /mnt/
      remoteMountDir: /
  image:
    imageType: personal
    imageUrl: 'ccr.ccs.tencentyun.com/afan-public/mini-vscode:v1.0.1@sha256:9d3d15e37f9923548dfa98c921e2dd71a31087be53c127915fe16c24a72ad448' 
  events:
    - apigw:
        parameters:
          protocols:
            - http
            - https
          environment: release
          endpoints:
            - path: /
              method: ANY
              enableCORS: true
              function:
                type: web
```


### 3. 部署

在 `serverless.yml` 文件所在的项目根目录，运行以下指令，将会弹出二维码，直接扫码授权进行部署：

```bash
serverless deploy
```

> **说明**：如果鉴权失败，请参考 [权限配置](https://cloud.tencent.com/document/product/1154/43006) 进行授权。

### 4. 查看状态

执行以下命令，查看您部署的项目信息：

```bash
serverless info
```

### 5. 移除

可以通过以下命令移除应用

```bash
serverless remove
```

### 账号配置（可选）

serverless 默认支持扫描二维码登录，用户扫描二维码后会自动生成一个 `.env` 文件并将密钥存入其中.
如您希望配置持久的环境变量/秘钥信息，也可以本地创建 `.env` 文件, 
把从[API 密钥管理](https://console.cloud.tencent.com/cam/capi)中获取的 `SecretId` 和`SecretKey` 填入其中.

> 如果没有腾讯云账号，可以在此[注册新账号](https://cloud.tencent.com/register)。

```bash
# 腾讯云的配置信息
touch .env
```

```
# .env file
TENCENT_SECRET_ID=123
TENCENT_SECRET_KEY=123
```
