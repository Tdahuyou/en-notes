# [0001. en-words 仓库简述](https://github.com/Tdahuyou/en-notes/tree/main/0001.%20en-words%20%E4%BB%93%E5%BA%93%E7%AE%80%E8%BF%B0)

- 📝 summary
  - en-words 仓库中存放了【qwerty-learner 英文单词数据源】解析后的所有单词数据。
  - 介绍了 en-words 中数据的来源。
  - 完整的【qwerty-learner 英文单词数据源】在 0003 中，将 0003 中的 sources 数据搬运到和脚本同级的 sources 目录中，然后再执行解析脚本。

## 🔗 links

- https://github.com/kajweb/dict
  - sources 中的数据来源于这个仓库。

## 📒 ntoes - en-words 目录说明

- en-words 目录下存放了解析后的所有单词数据。
- 单词按照统一的格式存储在一个个 .md 文件中，可以进行二次编辑，也可以扩展其它词汇，注意格式保持统一即可。
- 单词数据格式如下（以 wire 单词为例）：
  - 首先是单词的名称
  - 紧接着是单词的
    - 发言
    - 词义
    - 同根词
    - 近义词
    - 短语
    - 例句

```md
- wire
  - 发音
    - 英 `/waɪə/`
    - 美 `/'waɪɚ/`
  - 词义
    - n. 金属丝,电线
    - `thin metal in the form of a thread, or a piece of this`
    - v. 安装电线,发电报
    - `to send money electronically`
  - 同根词
    - adj.
      - `wired` 接有电线的；以铁丝围起的；极其兴奋的
      - `wireless` 无线的；无线电的
      - `wiry` 金属线制的；金属丝般的；坚硬的；瘦长结实的；（噪音）尖细的
    - adv.
      - `wirily` 铁丝状地
    - n.
      - `wireless` 无线电
      - `wiring` [电] 接线，架线；线路；金属线缝术
      - `wirer` 打电报者；用金属线缠结的工人；以铁丝网捕猎鸟兽者
      - `wiriness` 铁丝一样的形状
    - v.
      - `wired` 以金属丝装；打电报给（wire的过去分词）
      - `wiring` 装电线（wire的现在分词）
    - vi.
      - `wireless` 打无线电报；打无线电话
    - vt.
      - `wireless` 用无线电报与…联系；用无线电报发送
  - 近义词
    - n. [电]电线；金属丝；电报
      - `electric cord`
      - `electrical wiring`
    - vi. 打电报
      - `telegraph`
  - 短语
    - `steel wire` 钢丝
    - `wire rope` 钢丝索；钢缆， 钢索
    - `wire netting` 铁丝网；金属网
    - `wire rod` 线材；盘条；盘圆；钢丝筋条
    - `by wire` 用电报
    - `wire mesh` 金属丝网；铁丝网
    - `hot wire` n. [美俚]好消息；（不用钥匙起动点火装置的）短路点火；带电电线
    - `wire drawing` 拔丝；抽丝现象
    - `copper wire` 铜线
    - `stainless steel wire` 不锈钢丝
    - `electric wire` n. 电线
    - `wire in` [旧]努力工作；给…接上电源线
    - `wire cutting` 电火花线切割；钢丝切坯
    - `steel wire rope` 钢丝绳；钢丝索；钢缆
    - `welding wire` 焊丝；焊条
    - `barbed wire` 有刺铁丝网；棘铁丝
    - `iron wire` 铁丝；低碳钢丝
    - `live wire` 生龙活虎的人；通电的电线
    - `enameled wire` [瓷]漆包线；漆包铜线
    - `welded wire mesh` 电焊网；焊接钢丝网
  - 例句
    - `copper wire`
      - 铜丝
    - `a wire fence`
      - 铁丝网
```

- 单词的格式是参照数据源中的结构来定义的。
- 保持后续插入的新词汇格式的统一，这样后续编写统一的批处理脚本会比较方便，可以对所有词汇统一整理。

## 🤔 问：为什么要新建一个 en-words 仓库？直接将生成的单词放在当前的 en-notes 仓库中不行吗？

- en-notes.0001 中生成的单词数量很多（解析后默认有 2w 多个，后续学习过程中还会不断新增），体积有 150 多 MB，如果将单词放在 en-ntoes 中，由于单词数据和笔记数据混合在一起，会导致单词的查询成本变高。
- 将和笔记和单词数据分离开，让 en-words 仓库中仅存放单词文件，这样可以减少单词的查询成本、减少单词的维护成本。单词直接丢到根目录下，同时还有助于 url 的构建和复用。

## 💻 demo - 提取所有词汇的脚本

```js
/**
 * 1.js
 * 1.js 这个脚本是 24.10.26 时基于 en.0002 中的 demo/index4.js 编写的
 * 用于提取 sources 中的所有单词，将其汇总到 results 目录中。
 */
const fs = require('fs')
const path = require('path')

const SPACE_2 = '  ';
/**
 * 源目录名
 */
const SOURCE_FOLDER_NAME = 'sources'
/**
 * 目标目录名
 */
const RESULT_FOLDER_NAME = 'results'

const SUB_TITLE = {
  sentence: '例句',
  phone: '发音',
  usphone: '美',
  ukphone: '英',
  syno: '近义词',
  remMethod: '记忆',
  relWord: '同根词',
  phrase: '短语',
  trans: '词义',
}

let sourcesFolderPath = path.join(__dirname, SOURCE_FOLDER_NAME); // sources 目录的绝对路径
let resultsFolderPath = path.join(__dirname, RESULT_FOLDER_NAME); // results 目录的绝对路径

// 创建 results 目录（如果不存在）
if (!fs.existsSync(resultsFolderPath)) {
  fs.mkdirSync(resultsFolderPath, { recursive: true });
}

const JSON_FileList = fs.readdirSync(sourcesFolderPath)
  .filter(p => p.includes('.json'))
  .map(p => path.join(sourcesFolderPath, p)) // sources 目录下所有 json 文件的绝对路径

for (let i = 0; i < JSON_FileList.length; i++) {
  writeFile(JSON_FileList[i])
}

// 写入文件
function writeFile(file_path) {
  // JSON parse
  let data = JSON.parse(
    `[${fs.readFileSync(file_path, 'utf-8')}]`
    .replaceAll(/}\r\n/g, '},')
    .replaceAll(/},]/g, '}]')
  )

  data.forEach((it, i) => {

    // if (i > 1000) return;
    // if (i > 1000) return;

    if (/[\s-=?()0123456789]/.test(it.headWord)) return;

    const word = it.content.word.content

    let wordStr =
        '- ' +
        it.headWord.replace(/\//g, '\\') +
        '\n' +
        parsePhone(word) +
        parseTrans(word) +
        parseRemMethod(word) +
        parseRelWord(word) +
        parseSyno(word) +
        parsePhrase(word) +
        parseSentence(word);

    fs.writeFileSync(path.join(resultsFolderPath, `./${it.headWord.replace(/\//g, '\\')}.md`), wordStr)
  })
}

/* -- 发音部分 -- */
function parsePhone(word) {
  return `${SPACE_2}- ${SUB_TITLE.phone}
${SPACE_2}${SPACE_2}- ${SUB_TITLE.ukphone} \`/${word.ukphone}/\`
${SPACE_2}${SPACE_2}- ${SUB_TITLE.usphone} \`/${word.usphone}/\`
`
}

/* -- 词义部分 -- */
function parseTrans(word) {
  let text = ''

  // 拼接词义
  if (word.trans && word.trans.length > 0) {
    const trans = word.trans
    for (let i = 0; i < trans.length; i++) {
      const t = trans[i]
      if (t.pos && t.tranCn) text += `${SPACE_2}${SPACE_2}- ${t.pos}. ${t.tranCn.replace(/\s/g, '')}\n`
      if (t.tranOther) text += `${SPACE_2}${SPACE_2}- \`${t.tranOther}\`\n`
    }
  }

  return `${SPACE_2}- ${SUB_TITLE.trans}
${text}`
}

/* -- 记忆部分 -- */
function parseRemMethod(word) {
  return (word.remMethod && word.remMethod.val) ? `${SPACE_2}- ${SUB_TITLE.remMethod}
${SPACE_2}${SPACE_2}- ${word.remMethod.val.trim()}
` : ''
}

/* -- 同根词部分 -- */
function parseRelWord(word) {
  let text = ''

  if (word.relWord && word.relWord.rels && word.relWord.rels.length > 0) {
    const rels = word.relWord.rels
    for (let i = 0; i < rels.length; i++) {
      const r = rels[i];
      text += `${SPACE_2}${SPACE_2}- ${r.pos}.\n`
      text += r.words.map(w => `${SPACE_2}${SPACE_2}${SPACE_2}- \`${w.hwd}\` ${w.tran.trim()}`).join('\n') + '\n'
    }
  }

  return text ? `${SPACE_2}- ${SUB_TITLE.relWord}
${text}` : ''
}

/* -- 同近词部分 -- */
function parseSyno(word) {
  let text = ''

  if (word.syno && word.syno.synos && word.syno.synos.length > 0) {
    const synos = word.syno.synos
    for (let i = 0; i < synos.length; i++) {
      const s = synos[i];
      text += `${SPACE_2}${SPACE_2}- ${s.pos}. ${s.tran}\n`
      text += s.hwds.map(h => `${SPACE_2}${SPACE_2}${SPACE_2}- \`${h.w}\``).join('\n') + '\n'
    }
  }

  return text ? `${SPACE_2}- ${SUB_TITLE.syno}
${text}` : ''
}

/* -- 短语部分 -- */
function parsePhrase(word) {
  let text = ''

  if (word.phrase && word.phrase.phrases) {
    const phrase = word.phrase
    const phrases = phrase.phrases
    phrases.forEach(p => {
      text += `${SPACE_2}${SPACE_2}- \`${p.pContent}\` ${p.pCn} \n`
    })
  }

  return text ? `${SPACE_2}- ${SUB_TITLE.phrase}
${text}` : ''
}

/* -- 例句部分 -- */
function parseSentence(word) {
  let text = ''

  if (word.sentence && word.sentence.sentences) {
    const sentence = word.sentence
    const sentences = sentence.sentences
    sentences.forEach(s => {
      text += `${SPACE_2}${SPACE_2}- \`${s.sContent}\`\n${SPACE_2}${SPACE_2}${SPACE_2}- ${s.sCn}` + '\n'
    })
  }

  return text ? `${SPACE_2}- ${SUB_TITLE.sentence}
${text}
` : ''
}

/* 章节自测列表 */
function generateChapterMD(all_words, result_folder_path) {
  let checkString = '';
  all_words.map((h, i) => {
    const chapterNum = Math.floor(i / 20 + 1);
    if (i % 20 === 0) return `\n# Chapter ${chapterNum.toString().padStart(3, '0')}\n\n` + `- [ ] ${h}\n`
    else return `- [ ] ${h}\n`
  }).forEach((w, i) => {
    const chapterNum = Math.floor(i / 20 + 1);
    checkString += w;
    if (chapterNum % 10 === 0 && i === chapterNum * 20 - 1) {
      fs.writeFileSync(path.join(result_folder_path, `./${(chapterNum - 9).toString().padStart(3, '0')}~${chapterNum.toString().padStart(3, '0')}.md`), checkString)
      checkString = '';
    }
    if (i === all_words.length - 1) {
      fs.writeFileSync(path.join(result_folder_path, `./${(Math.floor(chapterNum / 10) * 10 + 1).toString().padStart(3, '0')}~${chapterNum.toString().padStart(3, '0')}.md`), checkString)
      checkString = '';
    }
  });
}

function clearResultFolder(result_folder_path) {
  emptyDir(result_folder_path)
  rmEmptyDir(result_folder_path)
  fs.mkdirSync(result_folder_path)
}

/**
 * 删除所有的文件(将所有文件夹置空)
 * @param {*} filePath
 */
function emptyDir(filePath) {
  try {
    const files = fs.readdirSync(filePath) // 读取该文件夹
    files.forEach((file) => {
      const nextFilePath = `${filePath}/${file}`
      const states = fs.statSync(nextFilePath)
      if (states.isDirectory()) {
        emptyDir(nextFilePath)
      } else {
        fs.unlinkSync(nextFilePath)
        // console.log(`删除文件 ${nextFilePath} 成功`)
      }
    })
  } catch (error) {
    // console.log(error)
    return
  }
}

/**
 * 删除所有的空文件夹
 * @param {*} filePath
 */
function rmEmptyDir(filePath) {
  try {
    const files = fs.readdirSync(filePath)
    if (files.length === 0) {
      fs.rmdirSync(filePath)
      // console.log(`删除空文件夹 ${filePath} 成功`)
    } else {
      let tempFiles = 0
      files.forEach((file) => {
        tempFiles++
        const nextFilePath = `${filePath}/${file}`
        rmEmptyDir(nextFilePath)
      })
      //删除母文件夹下的所有字空文件夹后，将母文件夹也删除
      if (tempFiles === files.length) {
        fs.rmdirSync(filePath)
        // console.log(`删除空文件夹 ${filePath} 成功`)
      }
    }
  } catch (error) {
    // console.log(error)
    return
  }
}
```