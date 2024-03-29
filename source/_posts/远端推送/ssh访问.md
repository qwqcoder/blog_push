---
title: ssh访问
tags:
	- ssh
	- rsa加密
layout: post
abbrlink: 6cede086
---

# ssh连接过程

## 密钥类型

![image-20240316001710619](../img/ssh%E8%AE%BF%E9%97%AE.assets/image-20240316001710619.png)

![image-20240316001727647](../img/ssh%E8%AE%BF%E9%97%AE.assets/image-20240316001727647.png)

## 认证流程

`SSH(Secure Shell)`是一种用于在网络上安全地进行远程登录和执行命令的协议。下面是 SSH 连接的简要流程：

1. **客户端发起连接请求：** 用户在本地计算机上的 `SSH` 客户端（比如 `OpenSSH` 或 `PuTTY`）输入远程服务器的地址和用户名，并发起连接请求。

2. **服务器端响应：** 服务器监听 SSH 连接请求，并在收到连接请求后响应客户端。

3. **密钥交换：** 客户端和服务器之间进行密钥交换，确保数据传输的安全性。这个过程通常包括以下步骤：
   - 客户端向服务器发送一个连接请求，并发送自己支持的加密算法列表。
   - 服务器从接收到的加密算法列表中选择一个加密算法，并发送自己的公钥给客户端。
   - 客户端使用服务器的公钥对一个随机生成的**会话密钥(对称密钥)**进行加密，并将加密后的会话密钥发送给服务器。
   - 服务器使用自己的私钥解密客户端发送的会话密钥。

4. **认证：** 客户端将用户凭据（如密码或 SSH 密钥）发送给服务器进行身份认证。如果凭据验证成功，客户端将被授权访问服务器。

5. **会话建立：** 认证成功后，客户端和服务器建立起安全的通信会话，可以在该会话中安全地传输数据。

6. **数据传输：** 客户端和服务器之间可以安全地传输数据，包括登录命令、文件传输、远程执行命令等操作。

7. **连接关闭：** 当用户退出登录或连接超时时，连接被关闭，SSH 会话结束。

总的来说，SSH 连接的流程涉及到密钥交换、身份认证和安全通信会话建立等步骤，确保了数据传输的安全性和可靠性。



# 区分authorized_keys与known_hosts文件

`authorized_keys` 和 `known_hosts` 都是与 SSH 密钥认证相关的文件，但它们在功能和用途上有所不同：

1. **authorized_keys：**
   - 位置：`authorized_keys` 文件位于服务器上的用户家目录下的 `.ssh` 目录中（通常是 `~/.ssh/authorized_keys`）。
   - 用途：`authorized_keys` 文件用于存储用户的公钥，以便服务器可以使用这些公钥来验证用户的身份。当用户尝试通过 SSH 连接到服务器时，服务器会检查该用户的 `authorized_keys` 文件，以确定是否允许连接。如果用户的公钥在 `authorized_keys` 文件中被发现，则服务器会允许用户连接，并使用相应的私钥进行身份验证。

2. **known_hosts：**
   - 位置：`known_hosts` 文件位于用户家目录下的 `.ssh` 目录中（通常是 `~/.ssh/known_hosts`）。
   - 用途：`known_hosts` 文件用于存储用户访问过的远程主机的公钥信息。当用户首次连接到一个远程主机时，SSH 客户端会将该主机的公钥保存到自己的 `known_hosts` 文件中。随后，当用户再次连接到同一台主机时，SSH 客户端会使用 `known_hosts` 文件中存储的公钥来验证主机的身份，以确保连接的安全性。

因此，`authorized_keys` 文件用于服务器端存储用户的公钥，用于用户的身份验证；而 `known_hosts` 文件用于客户端存储已知主机的公钥，用于验证远程主机的身份。这两个文件在 SSH 认证过程中扮演着不同的角色，但都是确保连接安全性的重要组成部分。