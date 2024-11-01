1. RabbitMQ是什么？

RabbitMQ是一个功能强大的开源消息代理（Message Broker），用于实现应用程序之间的消息传递。它采用AMQP（Advanced Message Queuing Protocol）协议来进行消息的发布和订阅。

2. RabbitMQ的基本概念

\- **消息**：在RabbitMQ中，消息是数据的基本单位，它由生产者发送给消息队列，然后由消费者从队列中接收和处理。

\- **生产者**：生产者是消息的发送方，它将消息发布到RabbitMQ的交换器（Exchange）中。

\- **消费者**：消费者是消息的接收方，它从RabbitMQ的队列中订阅并消费消息。

\- **交换器（Exchange）**：交换器负责接收来自生产者的消息，并将消息路由到队列。

\- **队列**：队列是消息的缓冲区，用于存储待消费的消息。

\- **绑定（Binding）**：绑定将交换器与队列关联起来，指定消息的路由规则。

3. RabbitMQ工作流程

1. 生产者将消息发布到交换器。

2. 交换器根据绑定规则将消息路由到相应的队列。

3. 消费者从队列中接收并处理消息。
   \#### 

4.  RabbitMQ安装与配置

   在centos7.6下：

   安装Erlang：RabbitMQ是使用Erlang编写的，所以首先需要安装Erlang。
   \# 安装Erlang

   ```
   sudo yum install epel-release
   sudo yum install erlang
   ```

   安装RabbitMQ：
   \# 添加RabbitMQ官方YUM仓库

   ```
   sudo tee /etc/yum.repos.d/rabbitmq.repo <<EOF
   [rabbitmq]
   name=rabbitmq
   baseurl=![img](file:///C:\Users\14404\AppData\Roaming\Tencent\QQTempSys\%W@GJ$ACOF(TYDYECOKVDYB.png)https://packagecloud.io/rabbitmq/rabbitmq-server/el/7/\$basearch
   enabled=1
   gpgcheck=1
   gpgkey=![img](file:///C:\Users\14404\AppData\Roaming\Tencent\QQTempSys\%W@GJ$ACOF(TYDYECOKVDYB.png)https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey
   EOF
   ```

   \# 安装RabbitMQ

   ```
   sudo yum install rabbitmq-server
   ```

   3. 启动RabbitMQ服务：
      \# 启动RabbitMQ服务

      ```
      sudo systemctl start rabbitmq-server
      ```

    \# 设置开机自启动

   ```
   sudo systemctl enable rabbitmq-server
   
   ```

   配置RabbitMQ：

   默认情况下，RabbitMQ管理员用户"guest"只能从本地登录。为了从远程访问RabbitMQ管理界面和API，你需要创建一个新的用户并赋予相应的权限。
   \# 创建新用户，例如admin，并设置密码

   ```
   sudo rabbitmqctl add_user admin your_password
   ```

   \# 赋予新用户管理员权限

   ```
   sudo rabbitmqctl set_user_tags admin administrator
   ```

   \# 授予新用户访问虚拟主机"/"的权限

   ```
   sudo rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
   ```

   5. 访问RabbitMQ管理界面：


   RabbitMQ默认的管理界面位于http://your_server_ip:15672/，在浏览器中输入该地址，使用刚刚创建的新用户登录即可访问管理界面。

可以访问[RabbitMQ官方网站](![img](file:///C:\Users\14404\AppData\Roaming\Tencent\QQTempSys\%W@GJ$ACOF(TYDYECOKVDYB.png)https://www.rabbitmq.com/download.html)下载和安装RabbitMQ。安装完成后，你可以访问[RabbitMQ管理界面](http://localhost:15672)来进行配置和监控。

\#### 5. RabbitMQ示例代码

以下是一个简单的C++示例代码，用于演示如何使用RabbitMQ进行消息传递：

```
#include <iostream>
#include <string>
#include <cstring>
#include <amqp.h>
#include <amqp_tcp_socket.h>

int main() {
    // 连接到RabbitMQ服务器
    amqp_connection_state_t conn = amqp_new_connection();
    amqp_socket_t *socket = amqp_tcp_socket_new(conn);
    amqp_socket_open(socket, "localhost", 5672); // 修改为你的RabbitMQ服务器地址和端口

    // 创建一个通道
    amqp_channel_open(conn, 1);
    amqp_get_rpc_reply(conn); // 确认通道是否成功打开

    // 声明一个队列
    amqp_queue_declare_ok_t *r = amqp_queue_declare(
        conn, 1, amqp_cstring_bytes("hello"), 0, 0, 0, 1,
        amqp_empty_table
    );
    amqp_get_rpc_reply(conn); // 确认队列是否成功声明

    // 发送消息
    std::string message = "Hello, RabbitMQ!";
    amqp_bytes_t message_bytes = amqp_cstring_bytes(message.c_str());
    amqp_basic_publish(conn, 1, amqp_empty_bytes, amqp_cstring_bytes("hello"),
                       0, 0, nullptr, message_bytes);
    amqp_get_rpc_reply(conn); // 确认消息是否成功发送

    std::cout << "Sent: " << message << std::endl;

    // 接收消息
    amqp_basic_consume(conn, 1, amqp_cstring_bytes("hello"), amqp_empty_bytes, 0, 1, 0,
                       amqp_empty_table);
    amqp_get_rpc_reply(conn); // 确认消费者是否成功创建

    while (1) {
        amqp_rpc_reply_t ret;
        amqp_envelope_t envelope;
        amqp_maybe_release_buffers(conn);
        ret = amqp_consume_message(conn, &envelope, nullptr, 0);
        if (AMQP_RESPONSE_NORMAL != ret.reply_type) {
            break;
        }

        std::string received_message(
            static_cast<char*>(envelope.message.body.bytes),
            envelope.message.body.len
        );
        std::cout << "Received: " << received_message << std::endl;

        amqp_destroy_envelope(&envelope);
    }

    // 关闭连接和释放资源
    amqp_channel_close(conn, 1, AMQP_REPLY_SUCCESS);
    amqp_connection_close(conn, AMQP_REPLY_SUCCESS);
    amqp_destroy_connection(conn);

    return 0;
}

```

