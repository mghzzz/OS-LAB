���������������
 
���������
   �����ָ��������������ϲ�ͨ�����ģ��ľ�������Ӳ��ϵͳ���ܵļ����ϵͳ��Hypervisorλ��Ӳ����ϵͳ֮�䣬�Ǵ�������������һ�����֡����������ͨ��Hypervisor�����ײ������ʩ��Դ���ṩ�໥������������ÿ��������ж�������һ��ϵͳ����װ��ͬϵͳ�������������ͬһ�������������С�
��������
������һ����Զ��������л�����λ��Ӳ���Ͳ���ϵͳ���Ϸ���ÿ��������������������ϵͳ���ں�ͨ���������ļ��Ŀ⡣����������ֻ�ܽ��ж�ȡ�ģ�ÿ������������ͨ���ض��ķ������й���д�롣�����ܹ���Ӧ����Ҫ�����л��������滷�������ݿ⻷���ȵȷ�װ����֧��Ӧ�����С�
�Ա�
���������������������Ŀ�������Ƶģ����Ƕ�Ӧ�ó���������Խ��и��룬��Ϊ��Ч��ʹ�ü�����Դ���Ӷ�������ԴЧ����ɱ�Ч�档���ǵ��������ڣ�ÿһ��������ж����Լ���һ������ϵͳ������������Ҫ��װ����ϵͳ��ͨ�������ں������������Ĳ���ϵͳ�����ʹ�������ر�ġ����ᡮ������Сһ������Ϊ��λ�������ٶȽϿ�ӵ�и��ߵ���Դʹ��Ч�ʣ�������Ϊ�����ںˣ�����������Ҳû���������ô�á�

����LXC Python API��д����
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
c.attach_wait(lxc.attach_run_command,["bash","-c",'echo "Ī����\n 1500012788" >Hello-Container'])

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
