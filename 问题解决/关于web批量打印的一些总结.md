### 关于web批量打印的一些总结

* 总述：web在批量打印上是存在难点的，因为js难以实现控制打印设置。现在常规的解决方案有三种。

  * 1、助插件。插件会调用打印接口，将打印数据塞入打印队列中从而实现不预览批量打印。我用过的较为好用的是lodop的第三方打印插件。但是一般这种插件也是收费较高的。
  * 2、使用报表软件。这种方式未尝试，但是学习成本较高。
  * 3、使用预览打印。这种方式本省js实现，只是将要打印的数据以分页的形式放在打印预览中来实现。

* 细说预览打印的方式

  * 思路：通过将需要打印的数据按照表格形式循环生成以分页的形式塞进新的页面中，然后调用打印，预览，实现批量打印

  * 关键点：使用css的page-break-after属性，这个属性强制分页

    ```html
    <div class="iframe-temp" style="display: none;">
            <table border="0" width="300" 
                   cellspacing="0" 
                   style="border-collapse:collapse;table-layout:fixed; margin-left: 10px;">
                <tr>
                    <td style=" width: 100px;">姓名</td>
                    <td class="real_name"></td>
                </tr>
                <tr>
                    <td>职称</td><td class="job"></td>
                </tr>
                <tr>
                    <td>单位</td><td class="unit"></td>
                </tr>
                <tr>
                    <td>
                        <img src="" class="barcode" 
                              alt="" width="300" height="100px"/>
                    </td>
                </tr>
            </table>
        </div>
    ```

    ```javascript
    $.ajax({
                    url:'url/'+(end_num-start_num+1),
                    type:'GET',
                    async:false,
                    scriptCharset: 'utf-8',
                    dataType:'json',
                    contentType: "application/x-www-form-urlencoded; charset=utf-8",
                    success:function(res){
                        if (res.status == 1){
                            for (i=0;i<res.data.length;i++){

                                //克隆模板生成新模板
                                let obj = $('.iframe-temp').clone();
                                obj.addClass('iframe iframe'+i);
                                obj.removeClass('iframe-temp');
                                obj.find('.barcode').attr('id','barcode'+i);
                                		    obj.find('.real_name').text(res.data[i].real_name);
                                obj.find('.job').text(res.data[i].job_title);
                                obj.find('.unit').text(res.data[i].company);
                                obj.find('.barcode').addClass('barcode'+i);

                                obj.appendTo('.wrapper');
                                			      createBarCode("#barcode"+i,res.data[i].invitation_sn)
                            }
                        }
                    }
                });
    ```

    ```javascript
    $(".iframe").each(function (i) {
        var pageNextStr = '<div style="page-break-after: always;"></div>';
        var pageNext = $(pageNextStr);
        pageNext.append($(this).html());
        body += pageNext[0].outerHTML;
    });

    win.document.body.innerHTML += body;
    win.print();
    win.close();
    ```

* 该实现手段的缺陷：

  * 因为dom的生成需要时间，因为调用了条漫生成的插件，在for循环完成后并没有形成所有的条码，进入预览中有条码缺失的情况，但是在取消打印再次进入预览时条码预览正常。
  * 预览打印数据量过大时预览时间会很长。这个暂无解决方案！！！

* 踩坑记

  在实现打印预览的时候使用过在本页面生成多个IFrame拼接到新的打印页面的方式，但是每一个IFrame都是若干次次http请求，在后期调试时发现，当量到200左右时出现504网关错误，nginx服务直接挂掉，必须重启服务才能正常访问，妈耶，这还是单台机器的请求，要是多台打印机多请求200都扛不住的哇！！！！于是乎只能改策略~~~逃。