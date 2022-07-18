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


## ROS 파일점색

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
- CMakeLists.txt 포함해야한다 