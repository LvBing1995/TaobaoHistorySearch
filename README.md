## 历史搜索流式布局##
 因为公司要求历史搜索做流式布局并且要支持展开收缩，然后这种网上果然有，别人的拿来用下，有点小bug进行了修复。

## 使用 ##
```javascript
  dependencies {
            implementation 'com.github.LvBing1995:TaobaoHistorySearch:1.0.1'
    }
```
```javascript
<com.lvbing.flowlayout.TagFlowLayout
            android:id="@+id/fl_search_records"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:paddingTop="@dimen/space_normal"
            app:is_limit="true"//是否启用展开收缩功能
            app:limit_line_count="2"//超过2行进行收缩
            app:max_select="2">//最大选择数adapter里有回调
            ```
```javascript
 //为标签设置对应的内容
        TagAdapter mRecordsAdapter = new TagAdapter<String>(recordList) {
            @Override
            public View getView(FlowLayout parent, int position, String s) {
                TextView tv = (TextView) LayoutInflater.from(MainActivity.this).inflate(R.layout.tv_history,
                        tagFlowLayout, false);
                //为标签设置对应的内容
                tv.setText(s);
                return tv;
            }
        };
        
        tagFlowLayout.setAdapter(mRecordsAdapter)
         .setOnTagClickListener(new TagFlowLayout.OnTagClickListener() {//点击事件
            @Override
            public void onTagClick(View view, int position, FlowLayout parent) {
                //清空editText之前的数据
                editText.setText("");
                //将获取到的字符串传到搜索结果界面,点击后搜索对应条目内容
                editText.setText(recordList.get(position));
                editText.setSelection(editText.length());
            }})
         .setOnLongClickListener(new TagFlowLayout.OnLongClickListener() {//长按事件
            @Override
            public void onLongClick(View view, final int position) {
                showDialog("确定要删除该条历史记录？", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        //删除某一条记录
                        mRecordsDao.deleteRecord(recordList.get(position));
                    }
                });
            }
         })
        .getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() { //view加载完成时回调
            @Override
            public void onGlobalLayout() {
                boolean isOverFlow = tagFlowLayout.isOverFlow();
                boolean isLimit = tagFlowLayout.isLimit();
                if (isLimit && isOverFlow) {//展开收缩
                    moreArrow.setVisibility(View.VISIBLE);
                } else {
                    moreArrow.setVisibility(View.GONE);
                }
            }
        });
   ```


