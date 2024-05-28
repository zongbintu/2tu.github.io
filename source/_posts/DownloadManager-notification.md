title: DownloadManager not work
date: 2015-11-25 11:25:09  
categories: Android  
tags: [DownloadManager]
---
问题：有用户反映点击版本升级后无响应(采用的DownloadManager)
思路：“应用程序管理”->“下载管理器”->“启用”，再点击“升级”，一切OK。但是不完美，所以我们需要检查是否启用，未启用则引导设置。    
![](DownloadMangerEnable.webp)  
解决办法：    
<!--more-->    
直接引用封装好的[UpgradeLibrary,点我](https://github.com/zongbintu/upgrade)

代码片断  
判断是否启用，未启用则弹出框让用户选择开启

	public final class DownloadManagerResolver {

    private static final String DOWNLOAD_MANAGER_PACKAGE_NAME = "com.android.providers.downloads";

    /**
     * Resolve whether the DownloadManager is enable in current devices.
     *
     * @return true if DownloadManager is enable,false otherwise.
     */
    public static boolean resolve(Context context) {
        boolean enable = resolveEnable(context);
        if (!enable) {
            AlertDialog alertDialog = createDialog(context);
            alertDialog.show();
        }
        return enable;
    }

    /**
     * Resolve whether the DownloadManager is enable in current devices.
     *
     * @param context
     * @return true if DownloadManager is enable,false otherwise.
     */
    private static boolean resolveEnable(Context context) {
        int state = context.getPackageManager()
                .getApplicationEnabledSetting(DOWNLOAD_MANAGER_PACKAGE_NAME);

        if (Build.VERSION.SDK_INT > Build.VERSION_CODES.JELLY_BEAN_MR2) {
            return !(state == PackageManager.COMPONENT_ENABLED_STATE_DISABLED ||
                    state == PackageManager.COMPONENT_ENABLED_STATE_DISABLED_USER
                    || state == PackageManager.COMPONENT_ENABLED_STATE_DISABLED_UNTIL_USED);
        } else {
            return !(state == PackageManager.COMPONENT_ENABLED_STATE_DISABLED ||
                    state == PackageManager.COMPONENT_ENABLED_STATE_DISABLED_USER);
        }
    }

    private static AlertDialog createDialog(final Context context) {
        TextView messageTextView = new TextView(context);
        messageTextView.setTextSize(16f);
        messageTextView.setText(R.string.downloadmanger_enable);
        return new AlertDialog.Builder(context)
                .setView(messageTextView)
                .setPositiveButton(R.string.ok, new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        enableDownloadManager(context);
                    }
                })
                .setCancelable(false)
                .create();
    }

    /**
     * Start activity to Settings to enable DownloadManager.
     */
    private static void enableDownloadManager(Context context) {
        try {
            //Open the specific App Info page:
            Intent intent = new Intent(android.provider.Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
            intent.setData(Uri.parse("package:" + DOWNLOAD_MANAGER_PACKAGE_NAME));
            context.startActivity(intent);
        } catch (ActivityNotFoundException e) {
            e.printStackTrace();

            //Open the generic Apps page:
            Intent intent = new Intent(android.provider.Settings.ACTION_MANAGE_APPLICATIONS_SETTINGS);
            context.startActivity(intent);
        }
    }
    }    
    

问题解决。    
类文件：[DownloadManagerResolver.java](https://github.com/2tu/ZYLibrary/blob/master/ZYLibrary/src/com/zongyou/library/app/upgrade/DownloadManagerResolver.java)



参考：http://www.samsung.com/hk_en/support/skp/htg/17649#none
http://stackoverflow.com/questions/21551538/how-to-enable-android-download-manager