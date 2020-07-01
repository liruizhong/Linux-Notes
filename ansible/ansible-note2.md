### ansible-playbook 显示命令返回结果

```yml
- hosts: vins-host  # host组
  serial: 20        # 执行并发数
  gather_facts: F   # 开启 debug
  remote_user: root # 执行用户
  tasks:
  - name: fenfa
    copy:
      src: /home/ruizhong.li/hostip/grep_rotate_date.sh
      dest: /tmp/
      owner: root
      group: root
      mode: 0644
  - name: doshell
    shell: nohup /bin/bash /tmp/grep_rotate_date.sh &
    ignore_errors: True
    register: grep_out   # 定义变量存储返回的结果
  - name: show           # 定义输出结果的task
    debug: var=grep_out.stdout verbosity=0 # grep_out.stdout 显示出的信息会看的更清晰点

```
