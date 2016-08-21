# IjkPlayerLibrary
B站开源播放器编译库(20160819),版本为Ijkplayer-k0.6.0  

### Ubuntu环境下编译B站开源视频播放器

#### 编译前准备  ####

	Linux环境：本地使用Ubuntu Kylin 15.10
	下载Linux：Java JDK，Android SDK，Android NDK，Git

	Ps：由于需要编译ffmpeg，所以在配置环境变量后要保证足够磁盘空间

	原来配置android-ndk-r11b 编译过程中报错：需要android-ndk-r10e及其之后的版本
	
#### 编译环境  ####

	Linux环境         Ubuntu Kylin 15.10
	Java 环境         JDK-1.8.0.66
	Android SDK
	Android NDK环境   android-ndk-r10e

#### 主要编译命令

[IjkPlayer：BiliBili站开源播放器](https://github.com/Bilibili/ijkplayer)

	git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android
	cd ijkplayer-android
	git checkout -B latest k0.6.0
	
	./init-android.sh
	
	cd android/contrib
	./compile-ffmpeg.sh clean
	./compile-ffmpeg.sh all
	
	cd ..
	./compile-ijk.sh all


#### 安装Git ####

	apt-get install git

#### 配置JAVA JDK ，Android SDK ，Android NDK环境变量 ####

*  sudo gedit  /etc/profile
*  添加
  
	    # Java Environment Path
	    export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_66 
	    export PATH=$JAVA_HOME/bin:$PATH 
	    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar	
	  
	    # set NDK env
	    export ANDROID_NDK=/home/wangmh/Desktop/android-ndk-r10e
	    export PATH=$PATH:$ANDROID_NDK
	    #NDKROOT=/home/wangmh/Desktop/android-ndk-r11b 
	    #export PATH=$NDKROOT:$PATH	
	
		# set Android_Home
		export ANDROID_HOME=/home/wangmh/Desktop/sdk
		export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH

*   Ctrl + S 保存，输入命令 sudo source /etc/profile 使其生效。


*   /etc/profile文件：
	
		# /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
		# and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).
		
		if [ "$PS1" ]; then
		  if [ "$BASH" ] && [ "$BASH" != "/bin/sh" ]; then
		    # The file bash.bashrc already sets the default PS1.
		    # PS1='\h:\w\$ '
		    if [ -f /etc/bash.bashrc ]; then
		      . /etc/bash.bashrc
		    fi
		  else
		    if [ "`id -u`" -eq 0 ]; then
		      PS1='# '
		    else
		      PS1='$ '
		    fi
		  fi
		fi
		
		# The default umask is now handled by pam_umask.
		# See pam_umask(8) and /etc/login.defs.
		
		if [ -d /etc/profile.d ]; then
		  for i in /etc/profile.d/*.sh; do
		    if [ -r $i ]; then
		      . $i
		    fi
		  done
		
		# Java Environment Path
		export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_66
		export PATH=$JAVA_HOME/bin:$PATH
		export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
		
		# set NDK env
		export ANDROID_NDK=/home/wangmh/Desktop/android-ndk-r10e
		export PATH=$PATH:$ANDROID_NDK
		#NDKROOT=/home/wangmh/Desktop/android-ndk-r11b
		#export PATH=$NDKROOT:$PATH
		
		# set Android_Home
		export ANDROID_HOME=/home/wangmh/Desktop/sdk
		export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH
		
		  unset i
		fi		

#### 相关命令 ####

* echo $PATH 
* 
		wangmh@ubuntu:~$ echo $PATH
		/home/wangmh/Desktop/sdk/tools:/home/wangmh/Desktop/sdk/platform-tools:/usr/lib/jvm/jdk1.8.0_66/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/wangmh/Desktop/android-ndk-r10e

* java -version
* 
		wangmh@ubuntu:~$ java -version
		java version "1.8.0_66"
		Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
		Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)

* ndk-build

		wangmh@ubuntu:~$ ndk-build
		Android NDK: Could not find application project directory !    
		Android NDK: Please define the NDK_PROJECT_PATH variable to point to it.    
		/home/wangmh/Desktop/android-ndk-r10e/build/core/build-local.mk:143: *** Android NDK: Aborting    .  Stop.

* 打包文件夹

		tar 压缩包输出路径  需要打包的文件夹路径
		tar -zcvf /home/wangmh/Desktop/ijk.tar.gz ijkplayer

#### IjkPlayer Proguard Rules
	
	-dontwarn tv.danmaku.ijk.media.player.**
	-keep class tv.danmaku.ijk.media.player.** { ; }
	-keep interface tv.danmaku.ijk.media.player.* { *; }

##### PS 

	//# required, enough for most devices.
    compile 'tv.danmaku.ijk.media:ijkplayer-java:0.6.0'
    compile 'tv.danmaku.ijk.media:ijkplayer-armv7a:0.6.0'

    //# Other ABIs: optional
    compile 'tv.danmaku.ijk.media:ijkplayer-armv5:0.6.0'

    /**
     * Error:Execution failed for task ':app:processDebugManifest'.
     > Manifest merger failed : uses-sdk:minSdkVersion 14 cannot be smaller than version 21 declared in library [tv.danmaku.ijk.media:ijkplayer-arm64:0.6.0] C:\Users\Administrator\Desktop\MHIjkPlayer\app\build\intermediates\exploded-aar\tv.danmaku.ijk.media\ijkplayer-arm64\0.6.0\AndroidManifest.xml
     Suggestion: use tools:overrideLibrary="tv.danmaku.ijk.media.player_arm64" to force usage
     */

    //compile 'tv.danmaku.ijk.media:ijkplayer-arm64:0.6.0'

    compile 'tv.danmaku.ijk.media:ijkplayer-x86:0.6.0'

    /**
     * Error:Execution failed for task ':app:processDebugManifest'.
     > Manifest merger failed : uses-sdk:minSdkVersion 14 cannot be smaller than version 21 declared in library [tv.danmaku.ijk.media:ijkplayer-x86_64:0.6.0] C:\Users\Administrator\Desktop\MHIjkPlayer\app\build\intermediates\exploded-aar\tv.danmaku.ijk.media\ijkplayer-x86_64\0.6.0\AndroidManifest.xml
     Suggestion: use tools:overrideLibrary="com.example.ijkplayer_x86_64" to force usage
     */

    //compile 'tv.danmaku.ijk.media:ijkplayer-x86_64:0.6.0'

    //# ExoPlayer as IMediaPlayer: optional, experimental
    compile 'tv.danmaku.ijk.media:ijkplayer-exo:0.6.0'





