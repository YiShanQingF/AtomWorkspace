## 目录
##### [Hadoop 安装 ](http://www.jianshu.com/p/8e57916790f2)
##### [单点启动&集群启动](http://www.jianshu.com/p/715dc8601065)
##### [访问 HDFS](http://www.jianshu.com/p/d8a5459c9f02)
##### [常用配置](http://www.jianshu.com/p/09bd95bdef0f)
##### [常用命令](http://www.jianshu.com/p/2a13831d0e79)


## 访问HDSF
### 浏览器访问
*给你一个地址自己去体会 http://192.168.8.150:50070/*

### linux 系统访问
**往HDFS文件系统上传文件&查看文件**
```
[root@node0 local]# ls
bin  games   hadoop-2.7.3.tar.gz  jdk-8u91-linux-x64.rpm  lib64    sbin   src
etc  hadoop  include              lib                     libexec  share
[root@node0 local]# hadoop fs -put ./hadoop-2.7.3.tar.gz /
[root@node0 local]# hadoop fs -ls /
Found 1 items
-rw-r--r--   3 root supergroup  214092195 2017-04-15 17:54 /hadoop-2.7.3.tar.gz
[root@node0 local]#

```

**创建目录**
```
[root@node0 local]# hadoop fs -mkdir -p /hello/world
[root@node0 local]# hadoop fs -mkdir  /hello
```
*递归初级&单个创建*
[参考资料 http://hadoop.apache.org](http://hadoop.apache.org/docs/r1.0.4/cn/hdfs_shell.html)


### java 访问
```
package com.qs;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URL;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FSDataOutputStream;
import org.apache.hadoop.fs.FileStatus;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.FsUrlStreamHandlerFactory;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IOUtils;

/**
 * @Package com.qs.HelloHDFS
 * @Description: HDFS 访问
 * @author QS
 * @date 2017年4月8日下午9:47:19
 * @version V1.0
 *  
 */
public class TestHDFS {


	public static void main(String[] args) throws Exception {

		/*URL url = new URL("http://www.baidu.com");
		InputStream inputStream = url.openStream();
		// hadoop.io.IOUtils 输入流			输出流	缓冲区	自动关闭
		IOUtils.copyBytes(inputStream, System.out, 4096, true);*/

		/*
		 * unknown protocol: hdfs	不认识 hdfs 协议
		 */
		// 设置流处理器
		URL.setURLStreamHandlerFactory(new FsUrlStreamHandlerFactory());
		URL url = new URL("hdfs://192.168.8.150:9000/test.txt");
		InputStream inputStream = url.openStream();
		// hadoop.io.IOUtils 输入流			输出流	缓冲区	自动关闭
		// IOUtils.copyBytes(inputStream, System.out, 4096, true);
		IOUtils.copyBytes(inputStream, System.out, 4096);

		System.out.println("----------");

		// 获取文件系统
		FileSystem fileSystem = getFileSystem();

		// 创建目录
		if (mkdirs_HDFS(fileSystem, "/Hello/World")) {
			System.out.println("/Hello/World 目录创建成功！");
		} else {
			System.out.println("/Hello/World 目录创建失败！");
		}

		// 判断文件&目录是否存在
		if (exists_HDFS(fileSystem, "/Hello/World")) {
			System.out.println("/Hello/World 存在！");
		} else {
			System.out.println("/Hello/World 不存在！");
		}

		// 删除文件
//		if (delete_HDFS(fileSystem, "/Download/")) {
//			System.out.println("/Hello 删除成功！");
//		} else {
//			System.out.println("/Hello 删除失败！");
//		}


		// 上传文件
		String localpath = "D:/tmpUpload/马士兵hadoop2.7入门_03.mp4";
		String hdfspath = "/Download/BaiduYunDownload/Hadoop/马士兵hadoop2.7入门_03.mp4";
//		uploadFile_HDFS(fileSystem, localpath, hdfspath);
//		uploadFileProgress_HDFS(fileSystem, localpath, hdfspath);

		// 下载文件
		hdfspath = "/Download/BaiduYunDownload/Hadoop/马士兵hadoop2.7入门_03.mp4";
		if(downloadFile(fileSystem, hdfspath, localpath, false)){
			System.out.println("文件下载成功！");
		} else {
			System.out.println("文件下载失败！");
		}

		// 获取系统文件元数据
		FileStatus fileStatus = getFileStatus(fileSystem, new Path(hdfspath));
		System.out.print(hdfspath);
		if (fileStatus.isDirectory()) {
			System.out.println(" 是目录！");
		}else if (fileStatus.isFile()) {
			System.out.println(" 是文件！");
		}
	}

	/**
	 * @Title: downloadFile
	 * @Description: HDFS 文件下载
	 * @param fileSystem
	 * @param hdfspath
	 * @param localpath
	 * @param flag
	 * @return
	 * @throws IOException  
	 */
	public static boolean downloadFile(FileSystem fileSystem, String hdfspath, String localpath, boolean flag) throws IOException {
		File file = new File(localpath);
		if(!flag){
			if(file.exists()){
				System.out.println("本地文件已存在！ " + localpath );
				return false;
			}
		}

		FileOutputStream out = new FileOutputStream(file);
		Path hdfsFilePath = new Path(hdfspath);
		FileStatus fileStatus = getFileStatus(fileSystem, hdfsFilePath);
		if(fileStatus.isFile()){
			InputStream in = fileSystem.open(hdfsFilePath);
			IOUtils.copyBytes(in, out, 4096, true);
			return true;
		} else {
			System.out.println("不是文件！" + hdfspath);
			return false;
		}
	}


	/**
	 * @Title: downloadFile
	 * @Description: HDFS 文件下载
	 * @param fileSystem
	 * @param hdfspath
	 * @param localpath
	 * @return  
	 * @throws IOException
	 */
	public static boolean downloadFile(FileSystem fileSystem, String hdfspath, String localpath) throws IOException {
		File file = new File(localpath);
		if(file.exists()){
			System.out.println("本地文件已存在！ " + localpath );
			return false;
		}
		FileOutputStream out = new FileOutputStream(file);
		Path hdfsFilePath = new Path(hdfspath);
		FileStatus fileStatus = getFileStatus(fileSystem, hdfsFilePath);
		if(fileStatus.isFile()){
			InputStream in = fileSystem.open(hdfsFilePath);
			IOUtils.copyBytes(in, out, 4096, true);
			return true;
		} else {
			System.out.println("不是文件！" + hdfspath);
			return false;
		}

	}

	/**
	 * @Title: getFileStatus
	 * @Description: 获取 HDFS 系统文件元数据
	 * @param fileSystem
	 * @param hdfsFilePath
	 * @return
	 * @throws IOException  
	 */
	public static FileStatus getFileStatus(FileSystem fileSystem, Path hdfsFilePath) throws IOException {

		// 获取文件元数据
		FileStatus fileStatus = fileSystem.getFileStatus(hdfsFilePath);
//		System.out.println(fileStatus.getPath());
//		// 权限
//		System.out.println(fileStatus.getPermission());
//		// 备份份数
//		System.out.println(fileStatus.getReplication());
//		System.out.println(fileStatus.getBlockSize());
//		System.out.println(fileStatus.getGroup());
//		System.out.println(fileStatus.getLen());
//		System.out.println(fileStatus.getModificationTime());
//		System.out.println(fileStatus.getOwner());
//		System.out.println(fileStatus.getClass());
//		
//		System.out.println(fileStatus.isDirectory());
//		System.out.println(fileStatus.isEncrypted());
//		System.out.println(fileStatus.isFile());
//		System.out.println(fileStatus.isSymlink());

		return fileStatus;
	}

	/**
	 * @Title: uploadFileProgress_HDFS
	 * @Description: 文件上传，有进度提示
	 * @param fileSystem
	 * @param string
	 * @return
	 * @throws IllegalArgumentException
	 * @throws IOException
	 */
	public static boolean uploadFileProgress_HDFS(FileSystem fileSystem, String localpath, String hdfspath) throws IllegalArgumentException, IOException {

		FSDataOutputStream out = fileSystem.create(new Path(hdfspath),true);
		File file = new File(localpath);
		FileInputStream in = new FileInputStream(file);
		byte[] buf = new byte[4096];
		int len = in.read(buf);

		long total = file.length();
		long up_total = 0;

		int progress_print = 0;
		while(len != -1){
			up_total += len;
			int progress = (int)((double)up_total/total * 100);
			if( progress%5 == 0 && progress != progress_print){
				progress_print = progress;
				System.out.print(progress + "%  ");
			}
			out.write(buf, 0, len);
			len = in.read(buf);
		}
		System.out.println();
		in.close();
		out.close();

		return true;
	}

	/**
	 * @Title: uploadFile_HDFS
	 * @Description: 文件上传，没有进度
	 * @param fileSystem
	 * @param path
	 * @return
	 * @throws IllegalArgumentException
	 * @throws IOException  
	 */
	public static boolean uploadFile_HDFS(FileSystem fileSystem, String localpath, String hdfspath) throws IllegalArgumentException, IOException {
		FSDataOutputStream out = fileSystem.create(new Path(hdfspath),true);
		FileInputStream fis = new FileInputStream(localpath);
		IOUtils.copyBytes(fis, out, 4096, true);
		return true;
	}

	/**
	 * @Title: delete_HDFS
	 * @Description: 删除文件&目录
	 * @param fileSystem
	 * @param path
	 * @return
	 * @throws IllegalArgumentException
	 * @throws IOException  
	 */
	public static boolean delete_HDFS(FileSystem fileSystem, String path) throws IllegalArgumentException, IOException {
		boolean flag = fileSystem.delete(new Path(path), true);
		return flag;
	}

	/**
	 * @Title: exists_HDFS
	 * @Description: 判断文件&目录是否存在
	 * @param fileSystem
	 * @param path
	 * @return
	 * @throws IllegalArgumentException
	 * @throws IOException  
	 */
	public static boolean exists_HDFS(FileSystem fileSystem, String path) throws IllegalArgumentException, IOException {
		boolean flag = fileSystem.exists(new Path(path));
		return flag;
	}

	/**
	 * @Title: mkdirs_HDFS
	 * @Description: 创建目录（递归覆盖创建） 注意：在创建文件夹是会覆盖原有的，因此在创建之前要判断文件夹是否存在
	 * @param string
	 * @return  
	 * @throws IOException
	 * @throws IllegalArgumentException
	 */
	public static boolean mkdirs_HDFS(FileSystem fileSystem,String path) throws IllegalArgumentException, IOException {
		boolean flag = fileSystem.mkdirs(new Path(path));
		return flag;
	}

	/**
	 * @Title: getFileSystem
	 * @Description: 获取 HDFS 文件系统
	 *  由于 windows 和 linux 的权限问题导致程序出错                                                                    
	 *	简单解决方案将 namenode 的 dfs.permissions.enabled 设置为 false (hdfs-site.xml)                              
	 *	设置好之后重启 namenode	hadoop-daemon.sh stop namenode                                                   
	 *	 						hadoop-daemon.sh start namenode                                          
	 * @return
	 * @throws IOException  
	 */
	public static FileSystem getFileSystem() throws IOException {
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://192.168.8.150:9000");
		// 设置备份份数，不设置默认为 3
		conf.set("dfs.replication", "2");
		// 获取远程的文件系统
		FileSystem fileSystem = FileSystem.get(conf);
		return fileSystem;
	}


}


```









[参考资料 http://hadoop.apache.org](http://hadoop.apache.org/docs/r1.0.4/cn/hdfs_shell.html)
[参考资料 http://mashibing.com/w/](http://mashibing.com/w/)
