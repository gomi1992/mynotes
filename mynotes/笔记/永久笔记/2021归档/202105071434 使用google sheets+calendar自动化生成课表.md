tags = #google #sheet #calendar #自动化 #notion #产品设计 #项目管理

# 起因
单位里没有相应的日历APP来查看哪天有课哪天没课，只有一个简单的excel，使用起来不是那么清晰明了。

数据如下表：

|班级构成|课程名称|学分|总学时|讲授学时|实验学时|教师|上课班号|上课人数|具体实验学时|周次|星期|节次|实验室|
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|B20262031(电信201) B20262032(电信202) B20262033(电信203) B20262051(智能201) B20262052(智能202)|[410703125]科技论文写作|1.0|16|8|8|[17107130]涂娟|003|65|6|6-8|二|[5-6节]|201|

# 半自动方案
使用excel公式生成相应的文本，然后手动去日历或者notion中黏贴。

优点：暂时没发现
缺点：
- 不直观，课表是按周次来计算的，跟校历有关，在普通日历上看，非常难受
- 工作量大
- 若有较大调整，则非常繁琐

# 自动化方案A
技术栈：
- python
- cardcav/caldav
- 手机客户端

python读取excel问题不大；python使用caldav库能够连接上大部分服务商（如qq、163等），少部分非常麻烦（icloud）；客户端问题，ios上客户端支持比较良好，PC和android支持较差。

由于客户端的体验较差，于是放弃了这个方案。

# 自动化方案B
技术栈
- google sheets 存储基础数据
- google calendar 提供日历及APP
- google app scripts 打通上面两者

## 具体实现
- calendar中，新建一个日历，便于分类管理
- 获取日历的id，https://developers.google.com/calendar/v3/reference/calendarList/list?hl=en，右侧try this api，点击执行，在执行结果中找到对应日历的id
- 新建sheets
- 在sheets中导入数据
- sheets中，工具->脚本编辑器
- 脚本编辑器中，脚本名无所谓，代码见文末完整代码小节
- 脚本编辑器中，服务添加sheets和calendar
- 在sheets中，工具->宏->导入，导入相关宏后就可以使用了

## 脚本使用
脚本具备插入删除星期标记的功能，方便查看周次，检查课表插入结果正确性。

选择一行或多行数据后即可插入或删除。

## 开发中遇到的问题
删除功能不好做，现阶段是在事件名后附上了学期信息，通过筛选找到相应的事件，以后可以使用加hash的方式来简化事件名。

sheets与脚本联调时不是很方便，需要在脚本编辑器中找到脚本执行页去看对应的执行日志。

# 感受
google全家桶挺香的，API支持多种语言，但是代码存放在云端还是方便。

对比notion，notion的使用体验很不错，但是他不能满足我的自动化的需求，同时也不对外提供API，无奈放弃notion自动化，同时放弃它作为课表管理工具的职能。

再对比传统工具自动化，客户端的体验不佳已经让人放弃。

google全家桶给人的感受：
- sheets和calendar基本功能齐全
- API丰富，文档齐全，同时提供try this api，方便快速的上手API
- app scripts功能强大，云端打通全家桶，编辑器支持自动补全，编码感觉良好，同时也具备必要的调试工具
- app scripts分享很强，尽管我没用上

当然也有缺点：
- sheets与脚本联调不是那么直观
- 仍需要一定的编码基础

改天再去了解下国内相关在线文档，跟google全家桶对比下。

# 自动化方案B 完整代码

```javascript
var calendarId = '72h5ur68og3gkf8qaek57pf4o8@group.calendar.google.com';

var MILLS_PER_MINUTE = 1000 * 60;
var MILLS_PER_HOUR = MILLS_PER_MINUTE * 60;
var MILLS_PER_DAY = MILLS_PER_HOUR * 24;

var TIME_TABLE = {
  1: {
    'start': 8 * MILLS_PER_HOUR + 20 * MILLS_PER_MINUTE,
    'end': 9 * MILLS_PER_HOUR + 5 * MILLS_PER_MINUTE
  },
  2: {
    'start': 9 * MILLS_PER_HOUR + 15 * MILLS_PER_MINUTE,
    'end': 10 * MILLS_PER_HOUR + 0 * MILLS_PER_MINUTE
  },
  3: {
    'start': 10 * MILLS_PER_HOUR + 20 * MILLS_PER_MINUTE,
    'end': 11 * MILLS_PER_HOUR + 5 * MILLS_PER_MINUTE
  },
  4: {
    'start': 11 * MILLS_PER_HOUR + 15 * MILLS_PER_MINUTE,
    'end': 12 * MILLS_PER_HOUR + 0 * MILLS_PER_MINUTE
  },
  5: {
    'start': 14 * MILLS_PER_HOUR + 0 * MILLS_PER_MINUTE,
    'end': 14 * MILLS_PER_HOUR + 45 * MILLS_PER_MINUTE
  },
  6: {
    'start': 14 * MILLS_PER_HOUR + 55 * MILLS_PER_MINUTE,
    'end': 15 * MILLS_PER_HOUR + 40 * MILLS_PER_MINUTE
  },
  7: {
    'start': 15 * MILLS_PER_HOUR + 50 * MILLS_PER_MINUTE,
    'end': 16 * MILLS_PER_HOUR + 35 * MILLS_PER_MINUTE
  },
  8: {
    'start': 16 * MILLS_PER_HOUR + 45 * MILLS_PER_MINUTE,
    'end': 17 * MILLS_PER_HOUR + 30 * MILLS_PER_MINUTE
  },
  9: {
    'start': 18 * MILLS_PER_HOUR + 15 * MILLS_PER_MINUTE,
    'end': 19 * MILLS_PER_HOUR + 0 * MILLS_PER_MINUTE
  },
  10: {
    'start': 19 * MILLS_PER_HOUR + 10 * MILLS_PER_MINUTE,
    'end': 19 * MILLS_PER_HOUR + 55 * MILLS_PER_MINUTE
  },
  11: {
    'start': 20 * MILLS_PER_HOUR + 05 * MILLS_PER_MINUTE,
    'end': 20 * MILLS_PER_HOUR + 50 * MILLS_PER_MINUTE
  },
  12: {
    'start': 21 * MILLS_PER_HOUR + 0 * MILLS_PER_MINUTE,
    'end': 21 * MILLS_PER_HOUR + 45 * MILLS_PER_MINUTE
  }
}

function testInput() {
  // addCorseEventToCalendar('2020--2021学年第二学期实验课表', '[410703125]科技论文写作', '[17107130]涂娟', '9', '二', '[5-6节]', '201', '2021-03-01T00:00:00+0800', true);
  // deleteCorseEventFromCalendar('2020--2021学年第二学期实验课表', '[410703125]科技论文写作', '[17107130]涂娟', '6-8', '二', '[5-6节]', '201', '2021-03-01T00:00:00+0800', true);
}

function menu_deleteWeekIndicatorFromCalendar() {
  var ss = SpreadsheetApp.getActiveSheet();
  var data = ss.getRange('P1').getValue();
  var section = ss.getRange('A1').getValue();
  Logger.log(data);
  Logger.log(section);
  deleteWeekIndicatorFromCalendar(section, data, 20);
}

function menu_addWeekIndicatorToCalendar() {
  var ss = SpreadsheetApp.getActiveSheet();
  var data = ss.getRange('P1').getValue();
  var section = ss.getRange('A1').getValue();
  Logger.log(data);
  Logger.log(section);
  addWeekIndicatorToCalendar(section, data, 20);
}

function menu_addCorseEventToCalendar() {
  var ss = SpreadsheetApp.getActiveSheet();
  var user = ss.getRange('Q1').getValue();
  var data = ss.getRange('P1').getValue();
  var section = ss.getRange('A1').getValue();
  Logger.log(data);
  Logger.log(section);
  Logger.log(user);
  ss.getActiveRange().getValues().forEach(function (row) {
    if (user == row[14]) {
      Logger.log(row[1]);
      addCorseEventToCalendar(section, row[1], row[6], row[10], row[11], row[12], row[13], data, true);
    }
  });
}

function menu_deleteCorseEventFromCalendar() {
  var ss = SpreadsheetApp.getActiveSheet();
  var user = ss.getRange('Q1').getValue();
  var data = ss.getRange('P1').getValue();
  var section = ss.getRange('A1').getValue();
  Logger.log(data);
  Logger.log(section);
  Logger.log(user);
  ss.getActiveRange().getValues().forEach(function (row) {
    if (user == row[14]) {
      Logger.log(row[1]);
      deleteCorseEventFromCalendar(section, row[1], row[6], row[10], row[11], row[12], row[13], data, true);
    }
  });
}

/**
 * 将第几周标记从日历中删除
 * firstWeekMondayDateString 第一周周一开始日期
 * weekCount 周数
 */
function deleteWeekIndicatorFromCalendar(section, firstWeekMondayDateString, weekCount) {
  for (var i = 0; i < weekCount; i++) {
    var q = 'jhun 第' + (i + 1).toString() + '周 ' + section;

    //列出全部事件
    var calEventItems = Calendar.Events.list(calendarId, { 'q': q })['items'];

    //删除相关事件
    for (var i in calEventItems) {
      var item = calEventItems[i];
      Calendar.Events.remove(calendarId, item['id']);
    }
  }
}

/**
 * 将第几周标记加入日历
 * firstWeekMondayDateString 第一周周一开始日期
 * weekCount 周数
 */
function addWeekIndicatorToCalendar(section, firstWeekMondayDateString, weekCount) {

  deleteWeekIndicatorFromCalendar(firstWeekMondayDateString, weekCount);

  //第一周周一日期
  var firstWeekMondayDateStart = new Date(firstWeekMondayDateString);
  firstWeekMondayDateStart = new Date(firstWeekMondayDateStart.getTime() + 1000 * 60 * 60 * 8);
  var firstWeekMondayDateEnd = new Date(firstWeekMondayDateStart.getTime() + 1000 * 60 * 60 * 10);

  //插入事件
  var event = {
    'summary': '',
    'start': {
      'dateTime': Utilities.formatDate(firstWeekMondayDateStart, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00'),
      'timeZone': 'Asia/Shanghai'
    },
    'end': {
      'dateTime': Utilities.formatDate(firstWeekMondayDateEnd, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00'),
      'timeZone': 'Asia/Shanghai'
    }
  };


  for (var i = 0; i < weekCount; i++) {
    event['summary'] = 'jhun 第' + (i + 1).toString() + '周 ' + section;

    var tmpDateStart = new Date(firstWeekMondayDateStart.getTime() + 1000 * 60 * 60 * 24 * 7 * i);
    var tmpDateEnd = new Date(firstWeekMondayDateEnd.getTime() + 1000 * 60 * 60 * 24 * 7 * i);
    event['start']['dateTime'] = Utilities.formatDate(tmpDateStart, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00');
    event['end']['dateTime'] = Utilities.formatDate(tmpDateEnd, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00');
    Calendar.Events.insert(event, calendarId);
  }
}

/**
 * 将课程事件从日历中删除
 * corseName 课程名
 * teacherName 教师名
 * corseWeeks 课程分布星期，例如：1-8，表示1-8周
 * corseDayInWeek 课程在周几上，例如：二
 * corseTimeInDay 课程在几点钟上，例如：[1-2节]
 * corsePlace 课程位置
 * firstWeekMondayDate 第一周周一日期
 * isRemind 是否提醒，暂未启用
 */
function deleteCorseEventFromCalendar(section, corseName, teacherName, corseWeeks,
  corseDayInWeek, corseTimeInDay, corsePlace,
  firstWeekMondayDateString, isRemind) {
  var summary = ' ' + corsePlace + ' 周' + corseDayInWeek + ' ' + corseTimeInDay + '节' + ' ' + corseName + ' ' + teacherName + ' ' + section;

  var corseWeekStart = parseInt(corseWeeks.split('-')[0]);
  var corseWeekEnd = parseInt(corseWeeks.split('-')[1]);

  for (var i = corseWeekStart; i <= corseWeekEnd; i++) {
    var q = '第' + i.toString() + '周' + summary;

    //列出全部事件
    var calEventItems = Calendar.Events.list(calendarId, { 'q': q })['items'];

    //删除相关事件
    for (var i in calEventItems) {
      var item = calEventItems[i];
      Calendar.Events.remove(calendarId, item['id']);
    }
  }
}

/**
 * 添加课程事件到日历中
 * corseName 课程名
 * teacherName 教师名
 * corseWeeks 课程分布星期，例如：1-8，表示1-8周
 * corseDayInWeek 课程在周几上，例如：二
 * corseTimeInDay 课程在几点钟上，例如：[1-2节]
 * corsePlace 课程位置
 * firstWeekMondayDate 第一周周一日期
 * isRemind 是否提醒，暂未启用
 */
function addCorseEventToCalendar(section, corseName, teacherName, corseWeeks,
  corseDayInWeek, corseTimeInDay, corsePlace,
  firstWeekMondayDateString, isRemind) {
  var summary = ' ' + corsePlace + ' 周' + corseDayInWeek + ' ' + corseTimeInDay + '节' + ' ' + corseName + ' ' + teacherName + ' ' + section;
  var location = corsePlace;
  var description = ' ' + corsePlace + ' 周' + corseDayInWeek + ' ' + corseTimeInDay + '节' + ' ' + corseName + ' ' + teacherName + ' ' + section;

  //第一周周一日期
  var firstWeekMondayDateStart = new Date(firstWeekMondayDateString);
  switch (corseDayInWeek) {
    case '一': corseDayInWeek = 0; break;
    case '二': corseDayInWeek = 1; break;
    case '三': corseDayInWeek = 2; break;
    case '四': corseDayInWeek = 3; break;
    case '五': corseDayInWeek = 4; break;
    case '六': corseDayInWeek = 5; break;
    case '日': corseDayInWeek = 6; break;
  }
  firstWeekMondayDateStart = new Date(firstWeekMondayDateStart.getTime() + corseDayInWeek * MILLS_PER_DAY);

  var event = {
    'summary': '',
    'location': location,
    'description': '',
    'start': {
      'dateTime': '2021-05-28T09:00:00-07:00',
      'timeZone': 'Asia/Shanghai'
    },
    'end': {
      'dateTime': '2021-05-28T17:00:00-07:00',
      'timeZone': 'Asia/Shanghai'
    },
    'reminders': {
      'useDefault': false,
      'overrides': [
        { 'method': 'email', 'minutes': 20 },
        { 'method': 'popup', 'minutes': 20 }
      ]
    }
  };

  corseTimeInDay = corseTimeInDay.substring(1, corseTimeInDay.length - 2);
  var corseTimeStart = parseInt(corseTimeInDay.split('-')[0]);
  var corseTimeEnd = parseInt(corseTimeInDay.split('-')[1]);

  if (corseWeeks.split('-').length == 1) {
    var corseWeekStart = parseInt(corseWeeks);
    var corseWeekEnd = parseInt(corseWeeks);
  } else {
    var corseWeekStart = parseInt(corseWeeks.split('-')[0]);
    var corseWeekEnd = parseInt(corseWeeks.split('-')[1]);
  }

  for (var i = corseWeekStart; i <= corseWeekEnd; i++) {

    var tmpDateStart = new Date(firstWeekMondayDateStart.getTime() + TIME_TABLE[corseTimeStart]['start'] + (i - 1) * 7 * MILLS_PER_DAY);
    var tmpDateEnd = new Date(firstWeekMondayDateStart.getTime() + TIME_TABLE[corseTimeEnd]['end'] + (i - 1) * 7 * MILLS_PER_DAY);

    event['summary'] = '第' + i.toString() + '周' + summary;
    event['description'] = '第' + i.toString() + '周' + description;
    event['start']['dateTime'] = Utilities.formatDate(tmpDateStart, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00');
    event['end']['dateTime'] = Utilities.formatDate(tmpDateEnd, 'Asia/Shanghai', 'yyyy-MM-dd\'T\'HH:mm:ss+08:00');
    Calendar.Events.insert(event, calendarId);
  }
}
```