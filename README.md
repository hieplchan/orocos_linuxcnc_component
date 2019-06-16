# Embedded Orocos KDL for 7DOF Robot Arm to LinuxCNC.
[Tutorial](https://docs.google.com/document/d/1HmaCXo7cQKbaq5FXnTHC77tiFhEHRa87daxEM2d-yVU/edit)  

- Moment testing of 1 TransZ link + 1 RotX link:
```
M = I.a_quay + m.a_dai.r = m.r^2.a_quay +  m.a_dai.r = F_dai.r
```

# 1. LinuxCNC HAL component compile
```
source /home/hiep/linuxcnc-dev/scripts/rip-environment
g++ -c -fPIC -L/usr/local/lib -I. -I/usr/local/include -I/usr/include/eigen3 -lorocos-kdl orocos_cnc.cpp -o orocos_cnc.o
g++ -fPIC -shared orocos_cnc.o -o liborocos_cnc.so
```

- Copy liborocos_cnc.so to /linuxcnc-dev/lib
- Copy liborocos-kdl.so, liborocos-kdl.so.1.4, liborocos-kdl.so.1.4 at /usr/local/lib to /linuxcnc-dev/lib
- Copy orocos_cnc.h to linuxcnc-dev/include

```
halcompile --install inverse_dynamics.comp
halrun
loadusr inverse_dynamics
```

# 2. LinuxCNC HAL component test
```
g++ -c -fPIC -L/usr/local/lib -I. -I/usr/local/include -I/usr/include/eigen3 orocos_cnc.cpp -o orocos_cnc.o -lorocos-kdl
g++ -fPIC -shared orocos_cnc.o -o liborocos_cnc.so

echo $LD_LIBRARY_PATH
LD_LIBRARY_PATH=/home/hiep/hiep_igh/orocos_linuxcnc_7dof:$LD_LIBRARY_PATH
LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
gcc -L. -L/usr/local/lib -I. -I/usr/local/include -I/usr/include/eigen3 test.c -lorocos_cnc -lorocos-kdl -o test
```
