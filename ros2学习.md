创建工作区
```
colcon build
```

创建接口功能包用来定义接口文件base_interfaces_demo
```
ros2 pkg create --build-type ament_cmake base_interfaces_demo
```

创建cpp01_topic这个功能包
```
ros2 pkg create cpp01_topic --build-type ament_cmake --dependencies rclcpp std_msgs base_interfaces_demo
```

编译cpp01_topic功能包
```
colcon build --packages-select cpp01_topic
```

执行cpp01_topic功能包中的 demo02_listener_str节点
```
. install/setup.bash 
ros2 run cpp01_topic demo02_listener_str
```


#### 通信
话题通信：一方服务，一方订阅。
服务通信：A发送目标，B执行并返回结果。
动作通信：例如导航，适用于长时间运行的任务。动作通信可以在请求响应过程中获
		取连续反馈。动作通信是建立在话题通信和服务通信之上的，目标发送实现是对服务通信的封装，结果的获取也是对服务通信的封装，而连续反馈则是对话题通信的封装。
参数服务：参数服务保存数据，类似于编程中“全局变量”的概念，可以在不同的节点
		之间共享数据。


#### 模板代码py:
```
import rclpy
from rclpy.node import Node

class MyNode(Node):
    def __init__(self):
        super().__init__("mynode_node_py")

def main():
    rclpy.init()
    rclpy.spin(MyNode())
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

#### 代码模板cpp
```
#include "rclcpp/rclcpp.hpp"

class Mynode : public rclcpp::Node
{
public:
    Mynode() : Node("mynode_cpp")
    {
        RCLCPP_INFO(this->get_logger(), "hello world!");
    }
};

int main(int argc, char const *argv[])
{
    rclcpp::init(argc, argv);
    auto node = std::make_shared<Mynode>();
    rclcpp::spin(node);
}

```

install(DIRECTORY launch DESTINATION shere/${PROJECT_NAME})