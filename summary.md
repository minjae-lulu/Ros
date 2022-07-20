#ros 튜토리얼


## ros 작업공간 만들기

-p로 한번에 2개의 폴더 만든다.
이동한다
init_workspace로 초기화해준다 -> (cmake파일 기본 생성)
```
$ mkdir -p /catkin_ws/src
$ cd /catkin_ws/src
$ catkin_init_workspace
```

cd로 위치 마꿔주고 src폴더 밖에서 
catkin_make로 빌드한다 (작업공간이 비어있긴 하지만 빌드) -> devel, build 폴더생성되고 devel폴더 안에넌 setup.*sh 파일 생성된다.  이 파일들을 쉘에 등록하면 이 작업공간이 ROS 환경의 최상위에 오버레이됩니다
```
$ cd ~/catkin_ws/
$ catkin_make
```


## ROS 파일검색

rospack find (package_name)
roscpp 패키지의 위치를 반환한다
```
$ rospack find roscpp
```


roscpp패키지가 있는 위치로 바로 이동한다. pwd로 위치확인이 가능하다.
```
$ roscd roscpp
```

log파일을 확인할수 있다.
```
$ roscd log
```

rosls [loaction name] -> 파일과 같은 폴더의 다른 파일들을 출력한다.<br>
roscpp_tutorials과 같이 있는 파일들이 출력되는것을 확인할수 있다.

```
$ rosls roscpp_tutorials
```

## ROS 패키지 작성
1. catkin 패키지의 조건
- package.xml 파일이 있어야한다 (패키지의 메타정보 제공)
- CMakeLists.txt 포함해야한다 <br><br>


2. 아래는 일반적인 작업공간 예제이다.
```
workspace_folder/        -- 작업공간
  src/                   -- 소스 폴더
    CMakeLists.txt       -- catkin이 제공하는 '최상위'의 CMake 파일
    package_1/
      CMakeLists.txt     -- package_1에 대한 CMakeLists.txt 파일
      package.xml        -- package_1에 대한 매니패스트
    ...
    package_n/
      CMakeLists.txt     -- package_n에 대한 CMakeLists.txt 파일
      package.xml        -- package_n에 대한 매니패스트
```
<br><br>
3. catkin 패키지 작성하기

std_msgs, roscpp, rospy에 대한 의존성을 가지는 'beginner_tutorials' 패키지를 만들기 catkin_create_pkg (패키지명) (의존성 패키지1) (의존성 패키지2) (...)

```
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```
이를 수행하면 package.xml과 CMakeLists.txt가 들어있는 beginner_tutorials폴더가 만들어 집니다. 



4. 패키지 의존성 (1차, 간접)
 catkin_create_pkg를 사용했을 때 몇 가지 의존성 정보를 입력했었습니다. 이들을 1차 의존성이라 하고 rospack 도구를 이용해 다시 확인 할 수 있다.
```
$ rospack depends1 beginner_tutorials 
```

많은 경우에 하나의 의존 패키지는 자체로도 의존 패키지를 가지고 있습니다. 일례로 rospy는 아래의 의존성을 가집니다.
```
$ rospack depends1 rospy
```

5. 패키지 사용자화

- package.xml 사용자화
package.xml는 새로 만든 패키지의 안에 있어야 한다.

  * description tag <br>
  ```  
  <description>The beginner_tutorials package</description> 
  ```
  패키지에 대한 설명을 적는 description tag칸이다.

  * maintainer tags <br>
  ```    
  7   <!-- One maintainer tag required, multiple allowed, one person per tag --> 
  8   <!-- Example:  -->
  9   <!-- <maintainer email="jane.doe@example.com">Jane Doe</maintainer> -->
  10   <maintainer email="user@todo.todo">user</maintainer> 
  ``` 
  가장 중요한 부분중 하나로, 패키지 관리자에 대한 정보가 입력하여야 한다. . 관리자의 이름은 태그의 몸체가 되고, 반드시 입력되어야 하는 웹메일 주소를 속성으로 가지고 있다.

  * license tags
  여기서 라이센스에 대한 정보를 반드시 입력해야 한다. 자주 쓰이는 라이센스로는 BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 등이 있다
  ```
  12   <!-- One license tag required, multiple allowed, one license per tag -->
  13   <!-- Commonly used license strings: -->
  14   <!--   BSD, MIT, Boost Software License, GPLv2, GPLv3, LGPLv2.1, LGPLv3 -->
  15   <license>TODO</license>
  ```

  * dependencies tags
  다음은 패키지의 의존성을 알려주는 태그의 모음들을 볼 수 있다. 의존성 목록들은 build_depend, buildtool_depend, run_depend, test_depend에 맞추어 나누어져 있다.
  우리는 catkin_create_pkg에서 std_msgs, roscpp, rospy를 의존성으로 정하였으므로 여기서는 아래와 같이 보인다.
  ```
  27   <!-- The *_depend tags are used to specify dependencies -->
  ...
  40   <build_depend>rospy</build_depend>
  41   <build_depend>std_msgs</build_depend>
  ```



## catkin 환경에서 작업공간 만들기
1. 작업공간 만들기
```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_workspace
```
위의 명령들을 실행하고 난 뒤에도 작업공간이 비어있지만(src폴더 안에는 어떤 패키지도 없고 CMakeLists.txt만 존재한다.) 이 작업공간을 "빌드"하는 것이 가능합니다.
```
$ cd ~/catkin_ws/
$ catkin_make
```
현재 폴더를 살펴보면 'build'와 'devel'폴더가 생긴 것을 알 수 있습니다. 'devel'폴더의 안에는 몇 가지 setup.*sh파일이 들어있습니다. 이 파일들을 쉘에 등록하면 이 작업공간이 ROS 환경의 최상위에 오버레이됩니다. 
```
$ source devel/setup.bash
```
위의 명령어를 통해 roslaunch하기전 .sh 파일들을 ros에 등록해준다.


## ROS Package 빌드하기
1. 패키지 빌드하기
