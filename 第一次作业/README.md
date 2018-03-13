虚拟机和容器技术
 
虚拟机技术
   虚拟机指在物理服务器的上层通过软件模拟的具有完整硬件系统功能的计算机系统。Hypervisor位于硬件和系统之间，是创建虚拟机必须的一个部分。虚拟机技术通过Hypervisor层抽象底层基础设施资源，提供相互隔离的虚拟机，每个虚拟机中都运行着一个系统。安装不同系统的虚拟机可以在同一个服务器上运行。
容器技术
容器是一个相对独立的运行环境，位于硬件和操作系统的上方，每个容器都共享主机操作系统的内核通常还包括文件的库。共享的组件是只能进行读取的，每个容器都可以通过特定的方法进行挂载写入。容器能够把应用需要的运行环境、缓存环境、数据库环境等等封装起来支持应用运行。
对比
首先容器与虚拟机技术的目的是相似的，都是对应用程序及其关联性进行隔离，更为高效地使用计算资源，从而提升能源效率与成本效益。他们的区别在于，每一个虚拟机中都有自己的一个操作系统，而容器不需要安装操作系统，通过共享内核来共享主机的操作系统，这就使得容器特别的‘’轻‘’，大小一般以兆为单位，启动速度较快拥有更高的资源使用效率，但是因为共享内核，容器隔离性也没有虚拟机那么好。

利用LXC Python API编写程序
import lxc
import sys

# Setup the container object
c = lxc.Container("oslab")
if c.defined:
    print("Container already exists", file=sys.stderr)
    sys.exit(1)

# Create the container rootfs
if not c.create(template="debian"):
    print("Failed to create the container rootfs", file=sys.stderr)
    sys.exit(1)

# Start the container
if not c.start():
    print("Failed to start the container", file=sys.stderr)
    sys.exit(1)

# Write the file
c.attach_wait(lxc.attach_run_command,["bash","-c",'echo "莫国辉\n 1500012788" >Hello-Container'])

# Stop the container
if not c.shutdown(30):
    print("Failed to cleanly shutdown the container, forcing.")
    if not c.stop():
        print("Failed to kill the container", file=sys.stderr)
        sys.exit(1)

# Destroy the container
if not c.destroy():
    print("Failed to destroy the container.", file=sys.stderr)
    sys.exit(1)
