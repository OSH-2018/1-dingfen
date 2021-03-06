## 					实验报告

班级：16计算机2班 			姓名：丁峰 			学号：PB16110386

#### **实验题目**：熟悉linux操作系统以及gitgithub的使用并学会调试操作系统的启动过程

#### **实验要求**：

​	在自己的电脑上安装linux操作系统，并且熟悉各种常用的命令、shell和脚本命令，学会使用git和github管理自己的代码库。使用调试跟踪工具追踪自己的操作系统（linux)的启动过程并找出其中至少两个关键事件。

#### **实验环境**：

​		主机操作系统：Linux16.04.1-Ubuntu SMP x86_64 GNU/Linux

​		gdb版本：gdb-7.11.1

​		qemu版本：2.11.1

​		测试的内核版本：4.15.13

​		

#### **安装linux操作系统并入门的学习git与github的实验步骤：**

安装linux并熟悉各种常用的命令、shell和脚本命令，学会使用git和github管理自己的代码库

​	一、安装linux操作系统，从ubuntu官网上下载ubuntu-16.04版本，然后将其映像文件刻录到U盘中，将电脑重新启动，进入BIOS，设置为U盘启动，开始安装ubuntu，进行磁盘分区等等操作，等待几十分中后安装完成。

​	二、熟悉linux的常用命令（例如：cp,cd,mv,rm,mkdir,rmdir等）学会使用--help和man查看帮助

​	三、学会shell命令，并能够写简单的脚本文件

​	四、学会使用Git以及Github，在自己电脑上创建相应的库，学会基本的git命令操作

​	五、完成学习任务后，创建自己的库，并上传编写完的hello_linux.sh的脚本文件。首先要在电脑上安装git程序和ssh协议，然后使用命令：git init建立自己的库。写完脚本文件后使用命令提交文件。

```
git add hello_linux.sh
git commit -m "xxxx"
git push remote origin
```

​	脚本文件解读：

```
#!/bin/bash			#该脚本首先会在屏幕上打印出Hello Linux然后会把|之前
echo Hello Linux    #的echo命令的重定流到output.txt中。
cat >output.txt
```

#### **追踪操作系统启动过程**：

​	一、从www.kernel.org上下载最新的内核版本4.15.13,并将下载的压缩包使用xz命令和tar命令进行解压缩。

具体命令：xz-cd linux-4.X.tar.xz | tar xvf -

​	二、使用makemrproper 和makeclean 进行清理然后使用makemenuconfig对内核编译选项进行选择，这里需要注意的是：在原来配置的基础上，makemenuconfig选中如下选项重新配置Linux，使之携带调试信息。具体操作是：在Kernelhacking——>compile the kernel with debug info中选Yes。否则，编译完成的内核无法提供调试信息！其余在内核中的.config文件的信息不要改动，直接点击Save然后Exit即可。最后使用make-j8 开始编译，等待约一小时后编译完成，开始下一步。

​	三、下载安装qemu	具体命令：apt-getinstall qemu-utils  然后cd进入arch/x86-64/boot中完成后使用命令：

​	qemu-system-x86_64-S  -append nokaslr -kernel./bzImage -m 1024 打开进入qemu开始模拟。

会出现如下窗口：这表示QEMU已经启动

![52258090154](/tmp/1522580901548.png)

​	四、进入后，按下Crtl+Alt+2进入命令行模式，输入gdbservertcp::1234，然后qemu便会等待宿主机的操作。此时，再打开一个终端，进入下载内核版本的目录，然后打开gdb，开始调试vmlinux，此时需要使用命令：gdbtarget remote localhost:1234 完成与qemu中的内核远程相连。

![img](data:image/png;base64,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fCZXx841v/tbVPP789pp9QbzmqeYEcNrjj/hXy89a1JB07f8ZX2Xopm7nn7NOPYwff4n//3XH47p+hv/8env7+198Terz/7GuyceevG3bv8XU8kMjEdfuG/1W0nNlS3sXvylH98eSXj86dan3ljzqNOslKo8e/vjy1e+s/ng86+45XwjkdTdLmX77V+59pE9ZuS5364968rTmud88OrfnBuNar7992P12/WpTC/X+bkAAOQUdhk9Xu9/7DaunZxosI6oRjJpHLi/NtnTlf5HVcOm63u7da+VW3mdHK/Xs3Hj5oPnzdVUrWToqunbQ1PWMj6rA2hTsG1vW/kF7YTc8fDf77jq58kvXrJk6viFp75nYd+MntdX/fgnj+6OpXy5O3AKuAIeTXUHM7cfGTHDDLiikY7nfvjtNR49FQw0q93JbOL6GrxuNd69cdUV39p90QWnHz9/gpVJye5dL6975M67Ho94XREl/4Rz4RleVYnryR1/WvfGksNaJzekr/JKhndteHbt7+5ca/p1r6Ktu+nGGf/vw8uPmOQNemO7evwBdZJuvH779d9oX3HR8sVzmlxK764X1676r3ue0wLNuU8Psb3tysRx1nZue+YPN9+yNuVvVOKlZ2lBd0hR1//y2m/uOO+Dy46ZP9FrxW2s/c2tUU1TzYag+vKt37p29/kXLlt08Ph03MbDb29/7R/tBUPZXDkFAJUoTFxdd3Um/V/dqVw2ITxHz8RLboHM8XZTwvODtkCX2+fV+51StQqmkqnXX9s4ffo0q8Oq5GVj9luiYrH49u3brV6XXqOC9oyQT9297o7PPb1q/qITPvCJDywKKokNd37l+jXbEtpBIS1pRoPB1P988eP3Jgtr83sVdyC88sKLDUWd3qRGXAG/0utvTPeOk0oiGFIf/9onHjZSIZ/iD4QaFKN3y59/8s3VP0pkv6hRDXjUxoA3obr69wbN/NDtiqrN2r6erg133/TXOxKJvu6vqvo8anPAk1B1j9KjJLf97rsrf5tIX9Klatq0Rj2qKaFAcsOaW1c+8BMjlV4+6NUa/N6o4tpfcfzFn6284aVo+gsjPeqEoCeiuO1mxdN95FgwoG945LarHvxFMtMITdMafabuC1q7PBRwvbzm11994GfZWar1OcSlmopmuPK2isQFgEoUfq+yxe1xhzXta3v1d3h6l3ijczyJBjUV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FwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASCBxAQCQQOICACCBxAUAQAKJCwCABBIXAAAJJC4AABJIXAAAJJC4AABIIHEBAJBA4gIAIIHEBQBAAokLAIAEEhcAAAkkLgAAEkhcAAAkkLgAAEggcQEAkEDiAgAggcQFAEACiQsAgAQSFwAACSQuAAASSFwAACSQuAAASNDvvfdewzASiYT1/2Tvy7+8+sbXF37qig+26qk+yWTSNM1UMhEzFF1T0pMimx9d9Zx58NHHHD6rSVfM/azqMv8mOreuf/75DdFDz1k2L6Aqqa7X1/7htanLly8MqUp2seySuUbkPx4yxr6/r3kyccI5i8e7sivt+tttv3ik+6gPX3LGFHeJ5eNbH7z5no3jzvz4hUc2qAcmJ3asufnOf3gXnHbOsqMP8lTRDjPR3W0GGzyq82LJWG80qWiqS/d63Vr/lr39yoaeqYe2Ng3Fx6VU5/O33by2bfa5l547e6DNS0U7I3pTcFDNSOzbvs8/rcU/wO44sM7u1596PjL32MOnBvL2SmLbQz+9a0PL+z+94mBvdooZ2bj67j91jpsxa9bMmTNnTG72aqUrrBXree0xgyHb5zX7eutd9M+fOH1yNXvM7N3w3z9/aEsy/VjzNU6YNO2QE087dnrZOw5AmqqqxY+LJ+bPysp/nP1T07Ts4+wDLcN64HK5tP10Xbf+78qwHv8fOlwMknUDFcAAAAAASUVORK5CYII=)	

​	五、设置断点，例如，使用命令gdbbstart_kernel在start_kernel()这个函数开始段设置断点，程序运行到这一步便会停止，此时按下c键并<Enter>，会发现如下gdb错误：

```
Remote‘g’  packet reply is too long
```

​	错误产生后，用如下方法修正即可：

![52258096971](/tmp/1522580969710.png)

​	六、于是，现在就可以再次设置断点进行下一步的调试。



#### 实验结果：

​	一、首先查看一下，当计算机在还未有任何命令执行时，寄存器中初始化的值：

![52258107239](/tmp/1522581072397.png)

​	
​	
​	

​	二、设置断点运行到start_kernel()函数后，所有寄存器的值如下：

![52258109850](/tmp/1522581098503.png)

​	

​	栈中保存的值如下：

![52258112386](/tmp/1522581123863.png)

​	

​	start_kernel()函数做了非常多的事情，这里仅列举几个。

​	首先执行了这一函数：set_task_stack_end_magic(&init_task);

建立了task_stack，然后做了一些初始化动作：

```
		smp_setup_processor_id();		
		debug_objects_early_init();	
		cgroup_init_early();
		local_irq_disable();

		early_boot_irqs_disabled= true;	
		boot_cpu_init();
		page_address_init();
		pr_notice("%s",linux_banner);
		setup_arch(&command_line);
		…………
```

​	

​	Trace也在这里完成初始化，使用了trace_init()函数

```
trace_init();
```

​	

​	time,context_tracking,call_function,console初始化。分别调用了函数：time_init() context_tracking_init()

call_function_init()等等。

```
		context_tracking_init();
		/*init some links before init_ISA_irqs() */
		early_irq_init();
		time_init();
		sched_clock_postinit();
		printk_safe_init();
		perf_event_init();
		profile_init();
		call_function_init();
		WARN(!irqs_disabled(),"Interrupts were enabled early\n");
		early_boot_irqs_disabled= false;
		local_irq_enable();
	
		kmem_cache_init_late();
		/*
		* HACK ALERT! This is early. We're enabling the console before
		* we've done PCI setups etc, and console_init() must be aware of
		* this. But we do want output early, in case something goes wrong.
		*/
		console_init();
		if(panic_later)
			panic("Toomany boot %s vars at `%s'", panic_later,
			     panic_param);
		lockdep_info();
```

 	

​	三、 在另一个地方设置一个断点kernel_thread()函数。

![52258158570](/tmp/1522581585703.png)		



​	可以看出pid是从kernel_thread()函数中获得的，目的是创建一个内核线程。在这里设置断点。在进入函数前，寄存器的值如下：![52258160898](/tmp/1522581608986.png)

​	

​	栈中的值：![52258162802](/tmp/1522581628025.png)



进一步深入发现，事实上kernel_thread函数调用了_do_fork()函数。

```
pid_tkernel_thread(int (*fn)(void *), void *arg, unsigned long flags)
	{
		return_do_fork(flags|CLONE_VM|CLONE_UNTRACED, (unsigned long)fn,
		(unsignedlong)arg, NULL, NULL, 0);
	}
```

​	

在_do_fork()函数的内部，pid是根据一系列的if语句执行不同的函数获取得到的。

```
		if(!IS_ERR(p)) {
			structcompletion vfork;
			structpid *pid;	
			trace_sched_process_fork(current,p);
	
			pid= get_task_pid(p, PIDTYPE_PID);
			nr= pid_vnr(pid);
	
			if(clone_flags & CLONE_PARENT_SETTID)
				put_user(nr,parent_tidptr);
	
			if(clone_flags & CLONE_VFORK) {
				p->vfork_done= &vfork;

				init_completion(&vfork);
				get_task_struct(p);
			}

			wake_up_new_task(p);
			/*forking complete and child started to run, tell ptracer */
			if(unlikely(trace))
				ptrace_event_pid(trace,pid);

			if(clone_flags & CLONE_VFORK) {
				if(!wait_for_vfork_done(p, &vfork))
					ptrace_event_pid(PTRACE_EVENT_VFORK_DONE,pid);
			}
			put_pid(pid);
		}else {
			nr= PTR_ERR(p);
		}
		returnnr;
```

#### 实验心得：

​	本次实验主要难点在于追踪操作系统启动过程。在实验过程中，在下载解压缩编译内核、使用qemu和gdb调试内核遇到了许多问题。遇到这些问题时应学会使用google解决，遇到实在没有办法的情况，应当主动询问同学老师。通过这次实验，学会了qemu+gdb的调试内核的方法，探索的过程十分艰辛，遇到很多千奇百怪的问题，但收获也很大，这次实验让我明白了计算机操作系统启动的步骤，十分感叹其过程复杂，工程浩大，实现起来有诸多不易。