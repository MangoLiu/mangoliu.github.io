#音频广告推送模型---FAQ
--------------------------------
以下是开发的过程的一些小问题，问题不大但挺多。记录与此，以防时间久了自己记不住：<br>
<strong>1 Q:为什么取登陆用户的播放数据时，还要额外再取一些wise端的数据？<br></strong>
<strong>A:</strong>因为在移动端的用户登陆比率太低了，直接去播放数据时几乎都是web的数据。为了保证数据均衡。<br>

<strong>2 Q:如何拿到的用户long-term属性？<br></strong>
<strong>A:</strong>通过大数据部的厄里斯魔镜接口，它有不同获取方式。后续针对在线实时的登陆用户的，我们要采取HTTP形式的。目前的训练集是通过工具形式，批处理获取的。<br>

<strong>3 Q:使用了哪些维度属性？<br></strong>
<strong>A:</strong>时间(event_hour),省份(event_province),城市(event_city),四端(terminal_type),歌曲(song_id),歌手(singer_name),设备(device_model),专辑(album_id)<br>

<strong>4 Q:本地程序读取文本时，乱码问题。<br></strong>
<strong>A:</strong>notepad++可以直接进行格式转换为ASNI。也可以直接在服务器端下载时进行转换。<br>


<strong>5 Q:以单属性“购物”为例，90%+的登录用户的long-term属性中都包含。导致训练时有问题，数据不平衡。<br></strong>
<strong>A:</strong>利用long-term中属性的可能权值。假如取N为阈值，小于这个值时，就认为这个用户不包含这个属性。这个N很好，可以控制数据平衡，还可以控制筛选出来的用户数。例如可以采用平均数，中位数，4分位数等等。<br>

<strong>6 Q:为什么要筛选掉过短的文本行。<br></strong>
<strong>A:</strong>没有什么行为的用户，不具有训练意义，算是杂质。<br>

<strong>7 Q:像5中提到的“购物”，但是一个用户中的属性，可能涉及到这个属性多次，并且权值不相同。例如：</strong><br>
>id:2848  id_type:kUserId age:25-34|81  gender:男|94 app_interest_normalized:购物|58 购物/母婴类购物倾向|25 购物/母婴类购物倾向/高端母婴购物|45 购物/电商购物/大麦网|4 购物/电商购物/京东|38 购物/电商购物/淘宝|35 购物/电商购物|38

<strong>A:</strong>取所有出现该属性的权值的平均值。<br>

<strong>8 Q:为什么把购物属性的阈值定为20？</strong><br>
<strong>A:</strong>这是根据人工算出的，采用1/4分位点。注：中位数的分位点为30。<br>

<strong>9 Q:SVM用户二分类，那么我如何利用其进行多分类？</strong><br>
<strong>A:</strong>通常情况是将多类分类器转换了多个二类分类器。<br>

<strong>10 Q:为什么web和wise端的单独分开来训练分类？</strong><br>
<strong>A:</strong>因为在web端和wise的日志打印不统一，同样的singer_name，前者是歌手id，后者是歌手名称。<br>

<strong>11 Q:libsvm格式的预处理，将字符串转换成数字。</strong><br>
<strong>A:</strong>使用awk将数据集中的省份列和城市列提取出来，分别存放到两个文件中。再以hashset遍历一遍，取出所有枚举值。这个工作做一次之后，几乎后续不必再做了。<br>

<strong>12 Q:中文格式问题。</strong><br>
<strong>A:</strong>在服务器端处理和显示需要UTF-8编码。在本地端使用eclipse处理时，需要转变成ANSI格式。<br>

<strong>13 Q:如何进行的归一化处理？</strong><br>
<strong>A:</strong>以数字id除以最大值；或是数组下标除以数组长度。<br>

<strong>14 Q:如何处理的特殊值？</strong><br>
<strong>A:</strong>具体参见Scale.java中对各个维度的处理，有注释。大多定为1或-1<br>

--------------------------------
######（转载本站文章请注明作者和出处 <a href="https://github.com/MangoLiu">MangoLiu</a> ，请勿用于任何商业用途）

