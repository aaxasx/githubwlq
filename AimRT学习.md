
#### AimRT源码
AimRT源码中build.sh配置如下：
```
#!/bin/bash

# exit on error and print each command
set -e

if [ -d ./build/install ]; then
    rm -rf ./build/install
fi

cmake -B build \
    -DCMAKE_BUILD_TYPE=Release \
    -DAIMRT_INSTALL=ON \
    -DCMAKE_INSTALL_PREFIX=./build/install \
    -DAIMRT_BUILD_TESTS=OFF \
    -DAIMRT_BUILD_EXAMPLES=ON \
    -DAIMRT_BUILD_DOCUMENT=ON \
    -DAIMRT_BUILD_PROTOCOLS=ON \
    -DAIMRT_BUILD_RUNTIME=ON \
    -DAIMRT_BUILD_CLI_TOOLS=ON \
    -DAIMRT_BUILD_PYTHON_RUNTIME=ON \
    -DAIMRT_USE_FMT_LIB=ON \
    -DAIMRT_BUILD_WITH_PROTOBUF=ON \
    -DAIMRT_USE_LOCAL_PROTOC_COMPILER=OFF \
    -DAIMRT_USE_PROTOC_PYTHON_PLUGIN=OFF \
    -DAIMRT_BUILD_WITH_ROS2=ON \
    -DAIMRT_BUILD_NET_PLUGIN=ON \
    -DAIMRT_BUILD_MQTT_PLUGIN=ON \
    -DAIMRT_BUILD_ZENOH_PLUGIN=ON \
    -DAIMRT_BUILD_ICEORYX_PLUGIN=ON \
    -DAIMRT_BUILD_ROS2_PLUGIN=ON \
    -DAIMRT_BUILD_RECORD_PLAYBACK_PLUGIN=ON \
    -DAIMRT_BUILD_TIME_MANIPULATOR_PLUGIN=ON \
    -DAIMRT_BUILD_PARAMETER_PLUGIN=ON \
    -DAIMRT_BUILD_LOG_CONTROL_PLUGIN=ON \
    -DAIMRT_BUILD_TOPIC_LOGGER_PLUGIN=ON \
    -DAIMRT_BUILD_OPENTELEMETRY_PLUGIN=ON \
    -DAIMRT_BUILD_GRPC_PLUGIN=ON \
    -DAIMRT_BUILD_ECHO_PLUGIN=ON \
    -DAIMRT_BUILD_PROXY_PLUGIN=ON \
    -DAIMRT_BUILD_PYTHON_PACKAGE=ON \
    $@

cmake --build build --config Release --target install --parallel $(nproc)  #$(nproc)改为1时，可单线程执行，使得报错可被捕捉。探究$(nproc)是什么
```

#### hello_world实例
编译：
```
cmake -B build
cd build
make -j$(nproc)
```

./：在当前目录下执行;
../跳转一个层级的目录，运行示例：
```
cnbot@cnrobot-pad:~/first_try/build/src/app/helloworld_app$ ./helloworld_app ../../../../src/install/cfg/helloword_cfg.yaml
```

编译完成后，也可将生成的可执行文件helloworld_app和配置文件helloworld_cfg.yaml拷贝到一个目录下，然后执行以下命令运行进程，观察打印出来的日志
```
./helloworld_app ./helloworld_cfg.yaml
```

绝对路径与相对路径区别
绝对路径：从根目录延伸至当前目录的路径
相对路径：从打开的目录开始延伸
根目录and主目录

/cmake/GetAimRT.cmake文件中，GIT_TAG版本改为你想引用的版本，版本号从相应项目中的tag标签中获取，目前最新版本为1.0.0-rc1


#### 配置aimrt_cli，使得在任何目录下皆可运行
cnbot@cnrobot-pad:~/AimRT/build/install/bin$ sudo cp -raf aimrt_cli /usr/local/bin
 