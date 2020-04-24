# Scratch3_language_translate
Scratch3 language translate 
### 一、Scratch3.0语言环境介绍<br>
1.1 Scratch3.0语言配置文件为：.../scratch-gui/src/reducers/locales.js。
```
import {addLocaleData} from 'react-intl';
import {localeData} from 'scratch-l10n';
import editorMessages from 'scratch-l10n/locales/editor-msgs';//语言文件从这里导出，包含中文，英文等几十个国家的语言
import {isRtl} from 'scratch-l10n';
addLocaleData(localeData);
const UPDATE_LOCALES = 'scratch-gui/locales/UPDATE_LOCALES';
const SELECT_LOCALE = 'scratch-gui/locales/SELECT_LOCALE';
```
1.2 修改语言的文件位置在：.../scratch-gui/node_modules/scratch-l10n/editor内，每个文件夹包含不同的模块的翻译文件。<br>
文件夹blocks包括block块的语言翻译文件；<br>
文件夹extensions包括扩展插件的语言翻译文件；<br>
文件夹interface包括Scratch3.0 GUI界面的语言翻译文件；<br>
文件夹paint-editor包括造型页面的语言翻译文件；<br>
每个文件夹中zh-cn.json文件为简体中文的语言翻译文件。<br>
### 二、语言环境配置<br>
2.1 使用cmder进入.../scratch-gui/node_modules/scratch-l10n路径，输入指令npm install安装依赖。<br>
2.2 进入scratch-l10n仓库：https://github.com/LLK/scratch-l10n, 下载整个文件，可以发现本地项目的文件夹scratch-l10n与下载的scratch-l10n相比缺少部分文件，将缺少的文件复制到本地项目的scratch-l10n内。（注：此处可以先将原来本地项目中的scratch-l10n备份一份，以免发生错误无法还原） <br>
### 三、修改语言<br>
注：Scratch3会自动选择与本地浏览器相同的语言，当浏览器的语言为中文时，Scratch3会显示语言为中文。<br>
例：修改posenet插件<br>
3.1 打开.../scratch-gui/node_modules/scratch-l10n/editor/extensions/zh-cn.json文件与posenet的index.js文件，需要注意index文件开头需要添加
```const formatMessage = require('format-message'); ```若没有则需要自行添加。 <br>
3.2 对ID号进行翻译：在index文件中找到getInfo()位置，修改每个积木块的语言,例如：
```
id: "pose2net",
            name: "pose2net",
```
在name后添加formatMessage()，相当于给这个name附加了一个id号pose2net.categoryName，这个id号下默认为Pose2net。<br>
```
id: 'pose2net',                                                        
            name: formatMessage({
                id: 'pose2net.categoryName',
                default: 'Pose2net',
                description: 'Label for the posenet extension category'
            }),
```
修改好index后，复制ID号pose2net.categoryName，到.../scratch-gui/node_modules/scratch-l10n/editor/extensions/zh-cn.json文件中，在文件中添加：<br>
```
"pose2net.categoryName":"人体姿态检测"
```
修改好后，保存两个文件，使用cmder进入.../scratch-gui/node_modules/scratch-l10n路径下输入指令npm run build，语言即可生效。<br>
3.3 对积木块进行翻译：在index文件getInfo()中找到opcode()，在text位置也加入formatMessage()语句如下：<br>
```
opcode: 'setconfidence',
                    blockType: BlockType.COMMAND,
                    text:formatMessage({
                        id: 'pose2net.setconfidence',
                        default: 'set confidence to [CONFIDENCE]',
                        description: 'set the confidence for drawing '
                    }),
```
此时表示setConfidence这个积木块的ID号为pose2net.setConfidence，默认显示为’set confidence to [CONFIDENCE]’。复制ID号到zh-cn.json文件中添加翻译：<br>
```
"pose2net.setconfidence":"设置置信度为 [CONFIDENCE]"
```
注意如果使用了数组，则需要在zh-cn.json文件中按照index.js文件中的书写，之后还需要单独对数组进行翻译。保存后使用npm run build即可生效。<br>
3.4 对数组进行翻译：例如[RESULTS]数组，在index中找到该数组的位置如下,在RESULT_INFO()中进行修改。（注：由于CONFIDENCE数组的类型为NUMBER，不需要进行翻译。）<br>
对数组中每一个选项的name后都添加一个formatMessage()语句，每一个选项都需要一个ID号进行修改。<br>
```
get RESULTS_INFO() {
        return [
            {
                name: formatMessage({
                    id: 'pose2net.leftup',
                    default: 'left-up',
                    description: 'Option for the left up'
                }), 
                value: RESULTS.LEFTUP
            },
```
复制ID号pose2net.leftup，在zh-cn.json文件中添加：<br>
```
"pose2net.leftup":"左边升起"
```
保存后使用npm run build命令即可生效。至此，对于index中需要翻译的内容已经全部翻译完成。<br>
3.5 对修改GUI界面进行翻译：打开.../scratch-gui/src/lib/libraries/extensions/index.jsx文件，找到添加的GUI内容如下：<br>
```
{
        name: (
            <FormattedMessage
                defaultMessage="pose2net"
                description="Name for the 'pose2net' extension"
                id="gui.extension.pose2net.name"
            />
            ),
            extensionId: 'pose2net',
            iconURL: pose2netImage,
            insetIconURL: pose2netInsetImage,
            description:(
                <FormattedMessage
                    defaultMessage="pose2 detection."
                    description="Description for the 'pose2net' extension"
                    id = "gui.extension.pose2net.description"
                />
                ),
                featured: true
    },
```
其中gui.extension.pose2net.name和gui.extension.pose2net.description为所需的两个ID号。
在editor文件夹内打开interface文件夹，在zh-cn.json文件中添加：<br>
```
"gui.extension.pose2net.name":"姿态检测",
"gui.extension.pose2net.description":"与姿态检测相关的插件"
```
保存后输入npm run build即可生效。<br>
