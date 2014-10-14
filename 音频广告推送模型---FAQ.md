#音频广告推送模型---FAQ
--------------------------------
<strong>Q:</strong>为什么取登陆用户的播放数据时，还要额外再取一些wise端的数据？<br>
<strong>A:</strong>因为在移动端的用户登陆比率太低了，直接去播放数据时几乎都是web的数据。为了保证数据均衡。<br>

<strong>Q:</strong>如何拿到的用户lon-term属性？<br>
<strong>A:</strong>通过大数据部的厄里斯魔镜接口，它有不同获取方式。后续针对在线实时的登陆用户的，我们要采取HTTP形式的。目前的训练集是通过工具形式，批处理获取的。<br>

<strong>Q:</strong>使用了哪些维度属性？<br>
<strong>A:</strong>时间(event_hour),省份(event_province),城市(event_city),四端(terminal_type),歌曲(song_id),歌手(singer_name),设备(device_model),专辑(album_id)<br>

<strong>Q:</strong>本地程序读取文本时，乱码问题。<br>
<strong>A:</strong>notepad++可以直接进行格式转换为ASCII。也可以直接在服务器端下载时进行转换。<br>


<strong>Q:</strong>以单属性“购物”为例，90%+的登录用户的long-term属性中都包含。导致训练时有问题，数据不平衡。<br>
<strong>A:</strong>利用long-term中属性的可能权值。假如取N为阈值，小于这个值时，就认为这个用户不包含这个属性。这个N很好，可以控制数据平衡，还可以控制筛选出来的用户数。例如可以采用平均数，中位数，4分位数等等。<br>

<strong>Q:</strong>为什么要筛选掉过短的文本行。<br>
<strong>A:</strong>没有什么行为的用户，不具有训练意义，算是杂质。<br>




--------------------------------
######（转载本站文章请注明作者和出处 <a href="https://github.com/MangoLiu">MangoLiu</a> ，请勿用于任何商业用途）

