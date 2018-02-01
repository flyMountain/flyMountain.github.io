---
layout: post
title: "AsyncTask理解"
date: 2017-12-09
description: "AsyncTask理解"
tag: AsyncTask
---

AsyncTask是轻量级的异步任务类,其主要是子线程处理任务得到的结果返回到主线程用于更新UI，以前很多的网络请求都是用AsyncTask来实现的。需要特别指出的一点是AsyncTask不适合进行特别耗时的后台任务（为什么？？ 个人猜测是因为AsyncTask一旦执行了doInBackground，就算调用取消方法，也会将doInBackground里面的代码执行完毕，才会停止，当进行特别耗时操作的时候会容易引起内存溢出）。

### AsyncTask组成
AsyncTask封装了Handler和Thread


### AsyncTask 提供了四个核心方法  
1.  onPreExecute()，在主线程中执行，是最先执行的方法  
2. doInBackground(Params... params) 在线程池中执行，params参数表示线程任务的入参  
3. onProgressUpdate(Progress... values)，在主线程中执行在doInBackground(Params... params)方法中通过publishProgress()方法进行调用  
4. onPostExecute(Result result) 在主线程中执行 入参为doInBackground方法的出参 主要用于更新UI  
除此之外还有onCancelled()方法 是在异步被取消时调用

`

	public class DownloadTask extends AsyncTask<String,Integer,Float>{


        @Override
        protected Float doInBackground(String... params) {
            publishProgress(1);
            return 0f;
        }

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
        }

        @Override
        protected void onPostExecute(Float aFloat) {
            super.onPostExecute(aFloat);
        }

        @Override
        protected void onProgressUpdate(Integer... values) {
            super.onProgressUpdate(values);
        }

        @Override
        protected void onCancelled() {
            super.onCancelled();
        }
    }`
    
###  关于AsyncTask的生命周期  
 关于AsyncTask存在一个这样广泛的误解，很多人认为一个在Activity中的AsyncTask会随着Activity的销毁而销毁。然后事实并非如此。AsyncTask会一直执行doInBackground()方法直到方法执行结束。一旦上述方法结束，会依据情况进行不同的操作
 
###  内存泄漏
还有一种常见的情况就是，在Activity中使用非静态匿名内部AsyncTask类，由于Java内部类的特点，AsyncTask内部类会持有外部类的隐式引用。由于AsyncTask的生命周期可能比Activity的长，当Activity进行销毁AsyncTask还在执行时，由于AsyncTask持有Activity的引用，导致Activity对象无法回收，进而产生内存泄露
### 结果丢失

另一个问题就是在屏幕旋转等造成Activity重新创建时AsyncTask数据丢失的问题。当Activity销毁并创新创建后，还在运行的AsyncTask会持有一个Activity的非法引用即之前的Activity实例。导致onPostExecute()没有任何作用

### 串行还是并行
在1.6以前，AsyncTask是串行执行任务 一个任务执行完成另一个才能执行。由于串行执行任务，使用多个AsyncTask可能会带来有些问题。所以这并不是一个很好的处理异步，从1.6到2.3Android团队决定让AsyncTask并行来解决1.6之前引起的问题，这个问题是解决了，新的问题又出现了。很多开发者实际上依赖于顺序执行的行为。于是很多并发的问题蜂拥而至
3.0到现在开发者可能并不喜欢让AsyncTask并行，于是Android团队又把AsyncTask改成了串行。当然这一次的修改并没有完全禁止AsyncTask并行。你可以通过设置executeOnExecutor(Executor)来实现多个AsyncTask并行