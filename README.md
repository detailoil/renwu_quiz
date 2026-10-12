<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>你在人情社会里，是哪一种“名场面”主角？</title>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { font-family: -apple-system, "PingFang SC", "Microsoft YaHei", sans-serif; background: #1a1410; color: #e8dcc8; display: flex; justify-content: center; min-height: 100vh; }
  .app { width: 100%; max-width: 480px; min-height: 100vh; background: #241c14; padding: 0 20px 40px; display: flex; flex-direction: column; }
  .page { display: none; flex-direction: column; width: 100%; }
  .page.active { display: flex; }
  .title-main { font-size: 22px; font-weight: 700; text-align: center; color: #d4af37; letter-spacing: 2px; margin-top: 40px; }
  .title-sub { font-size: 12px; text-align: center; color: #8a7d6b; margin-bottom: 30px; }
  .code-box { background: #2a2118; border: 1px solid #3d3020; border-radius: 12px; padding: 30px 20px; margin-top: 20px; text-align: center; }
  .code-box p { font-size: 14px; color: #b8a88a; margin-bottom: 20px; }
  .code-input { width: 100%; padding: 14px; font-size: 24px; letter-spacing: 12px; text-align: center; background: #1a1410; border: 2px solid #4a3a28; border-radius: 10px; color: #e8dcc8; outline: none; }
  .code-input:focus { border-color: #d4af37; }
  .btn { display: block; width: 100%; padding: 15px; font-size: 16px; font-weight: 600; border: none; border-radius: 10px; cursor: pointer; letter-spacing: 2px; }
  .btn-primary { background: linear-gradient(135deg, #d4af37, #b8942e); color: #1a1410; margin-top: 20px; }
  .btn-outline { background: transparent; color: #d4af37; border: 1px solid #d4af37; margin-top: 12px; }
  .msg-error { color: #e74c3c; font-size: 13px; margin-top: 12px; min-height: 20px; }
  .msg-success { color: #2ecc71; font-size: 13px; margin-top: 12px; min-height: 20px; }
  .progress-bar { width: 100%; height: 4px; background: #2a2118; border-radius: 2px; margin-bottom: 20px; }
  .progress-fill { height: 100%; background: #d4af37; width: 0%; transition: width 0.3s; }
  .q-index { font-size: 12px; color: #8a7d6b; text-align: center; margin-bottom: 10px; }
  .q-text { font-size: 17px; line-height: 1.7; color: #e8dcc8; margin-bottom: 24px; text-align: center; min-height: 60px; }
  .option { display: block; width: 100%; padding: 16px 18px; margin-bottom: 12px; background: #2a2118; border: 1.5px solid #3d3020; border-radius: 10px; color: #d8c8a8; font-size: 15px; text-align: left; cursor: pointer; }
  .option.selected { border-color: #d4af37; background: #3a2d1c; }
  .option .tag { display: inline-block; font-size: 11px; color: #d4af37; background: rgba(212,175,55,0.15); padding: 2px 8px; border-radius: 4px; margin-right: 8px; }
  .quiz-nav { display: flex; gap: 12px; margin-top: 20px; }
  .quiz-nav .btn { flex: 1; }
  #result-card { background: #2a2118; border: 1px solid #3d3020; border-radius: 16px; padding: 30px 20px; text-align: center; }
  .result-name { font-size: 28px; font-weight: 800; color: #d4af37; margin-bottom: 8px; }
  .result-sub { font-size: 14px; color: #b8a88a; margin-bottom: 20px; }
  .result-section-title { font-size: 13px; color: #d4af37; margin-bottom: 10px; text-align: left; font-weight: 600; }
  .result-desc { font-size: 14px; line-height: 1.8; color: #c8b898; text-align: left; margin-bottom: 20px; }
  .result-tags { display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; margin-bottom: 20px; }
  .result-tags span { font-size: 12px; color: #d4af37; border: 1px solid #4a3a28; padding: 4px 12px; border-radius: 20px; }
  .radar-wrap { width: 260px; height: 260px; margin: 0 auto 20px; }
  .result-actions { display: flex; flex-direction: column; gap: 12px; margin-top: 20px; }
  .action-row { display: flex; gap: 12px; }
  .action-row .btn { flex: 1; }
  .modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.7); justify-content: center; align-items: center; padding: 20px; z-index: 1000; }
  .modal-overlay.show { display: flex; }
  .modal { background: #2a2118; border: 1px solid #4a3a28; border-radius: 16px; padding: 30px 24px; max-width: 340px; text-align: center; }
  .modal h3 { color: #d4af37; margin-bottom: 12px; }
  .modal p { color: #b8a88a; font-size: 14px; line-height: 1.7; margin-bottom: 20px; }
  #capture-area { position: absolute; left: -9999px; top: 0; width: 375px; background: #1a1410; padding: 30px 20px; color: #e8dcc8; }
  #capture-area .cap-title { font-size: 20px; color: #d4af37; text-align: center; font-weight: 700; margin-bottom: 6px; }
  #capture-area .cap-sub { font-size: 11px; color: #8a7d6b; text-align: center; margin-bottom: 24px; }
  #capture-area .cap-name { font-size: 26px; color: #d4af37; text-align: center; font-weight: 800; margin-bottom: 6px; }
  #capture-area .cap-role { font-size: 13px; color: #b8a88a; text-align: center; margin-bottom: 20px; }
  #capture-area .cap-section { margin-bottom: 18px; }
  #capture-area .cap-section h4 { font-size: 13px; color: #d4af37; margin-bottom: 8px; }
  #capture-area .cap-section p { font-size: 13px; line-height: 1.8; color: #c8b898; }
  #capture-area .cap-tags { display: flex; flex-wrap: wrap; gap: 6px; justify-content: center; margin-bottom: 18px; }
  #capture-area .cap-tags span { font-size: 11px; color: #d4af37; border: 1px solid #4a3a28; padding: 3px 10px; border-radius: 16px; }
  #capture-area .cap-footer { text-align: center; font-size: 11px; color: #5a4a38; margin-top: 20px; border-top: 1px solid #2a2118; padding-top: 16px; }
  .toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%); background: rgba(212,175,55,0.95); color: #1a1410; padding: 12px 24px; border-radius: 8px; font-weight: 600; opacity: 0; transition: opacity 0.3s; pointer-events: none; }
  .toast.show { opacity: 1; }
</style>
</head>
<body>

<div class="app">
  <div id="page-code" class="page active">
    <div class="title-main">你在人情社会里</div>
    <div class="title-main" style="font-size:20px;">是哪一种“名场面”主角？</div>
    <div class="title-sub">12道中国式真实情境 · 测出你骨子里的处世剧本</div>
    <div class="code-box">
      <p>请输入客服提供的验证码<br>每个验证码可测试 3 次</p>
      <input type="tel" id="codeInput" class="code-input" maxlength="4" placeholder="请输入4位数字" inputmode="numeric">
      <div id="codeMsg" class="msg-error"></div>
      <button class="btn btn-primary" id="btnVerify">开始测评</button>
    </div>
  </div>

  <div id="page-quiz" class="page">
    <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
    <div class="q-index" id="qIndex"></div>
    <div class="q-text" id="qText"></div>
    <div id="optionsWrap"></div>
    <div class="quiz-nav">
      <button class="btn btn-outline" id="btnPrev">上一题</button>
      <button class="btn btn-primary" id="btnNext">下一题</button>
    </div>
  </div>

  <div id="page-result" class="page">
    <div id="result-card">
      <div class="result-name" id="resultName"></div>
      <div class="result-sub" id="resultSub"></div>
      <div class="radar-wrap"><canvas id="radarCanvas" width="260" height="260"></canvas></div>
      <div class="result-tags" id="resultTags"></div>
      <div class="result-section-title">📖 命运关键词</div>
      <div class="result-desc" id="resultKeyword"></div>
      <div class="result-section-title">🔍 深度解析</div>
      <div class="result-desc" id="resultAnalysis"></div>
      <div class="result-section-title">💡 改命建议</div>
      <div class="result-desc" id="resultAdvice"></div>
    </div>
    <div class="result-actions">
      <button class="btn btn-primary" id="btnSaveImg">保存结果长图</button>
      <div class="action-row">
        <button class="btn btn-outline" id="btnCopyLink">复制分享链接</button>
        <button class="btn btn-outline" id="btnRetake">再测一次</button>
      </div>
      <button class="btn btn-outline" id="btnReview" style="border-color:#e8c84a; color:#e8c84a;">好评有礼</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modalReview">
  <div class="modal">
    <h3>🎁 好评有礼</h3>
    <p>给店铺五星好评 + 10字以上带图评价，<br>联系客服发送好评截图，<br>即可获得 <strong style="color:#d4af37;">1 元返现</strong>。</p>
    <button class="btn btn-primary" id="btnCloseReview">我知道了</button>
  </div>
</div>

<div id="capture-area"></div>
<div class="toast" id="toast"></div>

<script>
const SUPABASE_URL = 'https://zsqovyeuwtpzfhujpcrp.supabase.co';
const SUPABASE_KEY = 'sb_publishable_con8cihK8x183LeHMBD5Wg_FwT6WYBq';
const supabaseClient = window.supabase ? window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY) : null;

const QUESTIONS = [
  { q: '过年亲戚聚餐，长辈当众问你工资和对象，你会：', options: ['笑着糊弄过去，转移话题。','反问对方孩子的情况，把皮球踢回去。','直接说“这是我私事”，低头吃菜。'] },
  { q: '你妈把你辛苦买的按摩椅送给了表姐，你的反应是：', options: ['心里委屈，但想想算了。','事后委婉跟妈妈提一下我也有点难过。','当场发作，问为什么不问我意见。'] },
  { q: '家族群长辈@你“不孝顺”视频，你会：', options: ['装没看见。','回个表情包。','直接甩辟谣文章回怼。'] },
  { q: '同事在领导面前把你的方案说成“团队一起做的”，你会：', options: ['当面不说什么，以后不合作了。','私下发消息给领导表明贡献。','立刻补充说核心数据是我做的。'] },
  { q: '领导饭局让你敬酒，但你吃了头孢，你会：', options: ['硬着头皮喝。','以茶代酒。','直接拒绝。'] },
  { q: '好朋友找你借钱，你不想借，会：', options: ['借一小部分，当送他了。','说手头紧，房贷压力大。','直接说不借钱给朋友。'] },
  { q: '和伴侣吵架，对方说“你要这么想我也没办法”，你会：', options: ['不说话了，心里觉得他不在乎我。','先冷静，等情绪好点再沟通。','气炸了，立刻掰扯清楚。'] },
  { q: '发现伴侣偷偷给前任点赞，你会：', options: ['自己难受，假装不知道，暗中观察。','开玩笑试探。','直接截图质问。'] },
  { q: '排队买奶茶被老人插队，你会：', options: ['心里气但忍了。','看情况，不方便就让，倚老卖老就说。','直接拍他肩膀指出来。'] },
  { q: '网购发错货，客服态度差，你会：', options: ['懒得折腾，凑合用。','好好沟通换货。','直接退款+差评。'] },
  { q: '发达的朋友总在炫耀还贬低你，你会：', options: ['慢慢疏远。','维持表面关系，心里降级。','当场怼回去。'] },
  { q: '有机会去大城市闯荡，但要放弃现在安稳生活，你会：', options: ['不去，梦想藏心里。','先去试半年。','去，哪怕头破血流也要试。'] }
];

const RESULTS = {
  'lin': { name:'林黛玉', title:'冷月葬花魂', source:'《红楼梦》', sub:'敏感、内耗、深情而孤独', tags:['内耗','共情','敏感','诗意'], keyword:'你总是先感受到世界的尖锐，再感受到它的温柔。', analysis:'你的人格底色是高敏感（HSP）。你能捕捉别人忽略的情绪细节，这让你活得很累，但也让你比任何人都更懂得什么是“共情”。在心理学上这叫“反刍思维”——反复咀嚼负面情绪，越嚼越苦。你不是玻璃心，你只是没学会把敏感变成盔甲。', advice:'把“反刍”变成“书写”。当你又反复想一件事时，把它写下来。写完了，你就不会那么疼了。先照顾自己的真实感觉，再去理解别人。', radar:[95,40,30,85,90] },
  'sun': { name:'孙悟空', title:'大闹天宫', source:'《西游记》', sub:'嫉恶如仇、边界感强', tags:['外化','反抗','正义','冲动'], keyword:'你的愤怒，是你最忠诚的铠甲。', analysis:'你受不了委屈，边界感极强。一旦有人越界，你会立刻反击，不计后果。心理学上叫“心理抗拒”——当自由被威胁时，你爆发的能量远超常人。你骨子里有不服输的野性，这在开疆拓土时是优势，但在讲究人情的社会容易伤人伤己。', advice:'真正的高手，是能控制自己愤怒的人。想拍桌子前，多等三秒问自己：“发火之后，能解决问题吗？”答案是否定的话，换一种聪明的方式反击。', radar:[40,98,30,50,60] },
  'xue': { name:'薛宝钗', title:'好风凭借力', source:'《红楼梦》', sub:'高情商、懂分寸、顾全大局', tags:['面子','圆融','通透','隐忍'], keyword:'你照顾了所有人的情绪，却唯独忘了自己。', analysis:'你情商极高，懂得察言观色，永远给足别人面子。宁愿自己吃亏，也不想场面难堪。这在中国文化里叫“面子协商”——你本能维护他人社会形象，以此换取群体和谐。你是别人眼中最靠谱的朋友，但只有你知道自己有多累。', advice:'“懂事”很珍贵，但不要变成讨好。学会在“给面子”和“护自己”之间划界限。你不需要为所有人的情绪负责。下一次，试着说“这件事我需要想想”。', radar:[70,30,98,90,50] },
  'wang': { name:'王熙凤', title:'机关算尽太聪明', source:'《红楼梦》', sub:'谋略过人、社交高手', tags:['谋略','关系','精明','强势'], keyword:'你能摆平所有事，但摆不平自己的心。', analysis:'你是天生的社交操盘手，懂得利用人情网络达成目的。社会学里有个词叫“社会资本”——人际关系本身就是可以投资和变现的资本。你把这种资本运用到极致，但代价是你必须一直端着，一直算计。', advice:'能力很强，但要注意“人情债”的边界。过度消耗关系，最后会众叛亲离。试着在某些关系里放下算计，做一个“不设防”的人。真正的安全感，来自不求回报的信任。', radar:[50,80,70,98,60] },
  'fu': { name:'福贵', title:'平静的绝望', source:'《活着》', sub:'隐忍、坚韧、在苦难中开出花', tags:['隐忍','坚韧','承受','生命力'], keyword:'你的一生都在承受，但你从未被打倒。', analysis:'你拥有中国人最底层的坚韧。不是没有痛感，而是习惯了把痛苦嚼碎了咽下去。你的人生底色是“熬”，但你熬出了生命的厚度。心理学上有“创伤后成长”——人在经历重大丧失后，依然可以重建生命意义。', advice:'坚韧令人敬佩，但你不是必须一直当顶梁柱。允许自己哭，允许自己求助，允许自己偶尔“撑不住”。你已经被命运锤了太多次，有资格活得更轻松。', radar:[90,20,60,40,85] },
  'guan': { name:'关羽', title:'身在曹营心在汉', source:'《三国演义》', sub:'忠义、重情、有担当', tags:['忠诚','义气','责任','固执'], keyword:'你的忠诚很珍贵，但别把它交给不值得的人。', analysis:'你重情重义，有强烈的归属感。一旦认定某个人或团队，你会誓死追随。心理学上的“社会认同理论”指出，个体自我价值很大程度来源于所属群体。你愿意为“我们”牺牲“我”，这是伟大，也是软肋。', advice:'忠诚很珍贵，但要警惕被不值得的人利用。选对“主公”比努力更重要。在付出之前，先问自己：这个人、这件事，配得上我的全力以赴吗？', radar:[60,70,70,85,50] },
  'fang': { name:'方鸿渐', title:'进退两难', source:'《围城》', sub:'聪明但犹豫、渴望但不敢', tags:['迷茫','犹豫','体面','内耗'], keyword:'你最大的敌人不是别人，是你自己的犹豫。', analysis:'你不缺聪明才智，但缺乏决断力。总在“将就”和“不甘心”之间摇摆。心理学上叫“决策疲劳”——面临太多选择时，反而陷入瘫痪。不是不想改变，是害怕选错。而害怕选错本身，就是最大的错误。', advice:'别想太多，先做选择。哪怕是错的，也比原地踏步强。人生不是考试，没有标准答案。迈出去的每一步，都会在未来某天连成一条线。', radar:[80,35,70,50,75] },
  'kuang': { name:'狂人', title:'救救孩子', source:'《狂人日记》', sub:'清醒、孤独、不愿妥协', tags:['清醒','批判','孤独','理想'], keyword:'你太早看到了真相，所以注定孤独。', analysis:'你极其清醒，看透了人情社会的虚伪与规则。你有时感到孤独，因为太早看到本质。你是那种在人群中突然沉默的人，因为知道很多话说了也没用。这种状态叫“知识分子的孤独”——认知水平决定了社交半径。', advice:'清醒是痛苦的，但也是自由的。你不需要让所有人理解你，只需要找到一两个同类。哪怕只有一个，你就不再孤独。看透真相后，依然选择热爱它。', radar:[75,60,40,40,95] }
};

let currentCode = '';
let currentAnswers = [];
let currentQuestionIndex = 0;

function $(s){ return document.querySelector(s); }
function $all(s){ return document.querySelectorAll(s); }
function showPage(id){ $all('.page').forEach(p=>p.classList.remove('active')); $(id).classList.add('active'); window.scrollTo(0,0); }
function showToast(m){ const t=$('#toast'); t.textContent=m; t.classList.add('show'); setTimeout(()=>t.classList.remove('show'),2000); }
function showModal(id){ $(id).classList.add('show'); }

$('#btnVerify').addEventListener('click', async function(){
  const input = $('#codeInput').value.trim();
  const msg = $('#codeMsg');
  if (!/^\d{4}$/.test(input)) { msg.textContent='请输入4位数字验证码'; msg.className='msg-error'; return; }
  if (!supabaseClient) { msg.textContent='网络异常，请刷新重试'; msg.className='msg-error'; return; }
  msg.textContent='正在验证...'; msg.className='msg-success';
  try {
    const { data, error } = await supabaseClient.from('codes').select('*').eq('code', input).single();
    if (error || !data) { msg.textContent='验证码无效，请检查后重新输入'; msg.className='msg-error'; return; }
    if (data.used_count >= data.max_uses) { msg.textContent='该验证码使用次数已用完，请联系客服'; msg.className='msg-error'; return; }
    await supabaseClient.from('codes').update({ used_count: data.used_count + 1 }).eq('code', input);
    currentCode = input;
    msg.textContent='验证成功，正在进入测评...'; msg.className='msg-success';
    setTimeout(()=>{ startQuiz(); }, 300);
  } catch(e) {
    msg.textContent='网络异常，请刷新重试'; msg.className='msg-error';
  }
});

function startQuiz() {
  currentAnswers = new Array(QUESTIONS.length).fill(-1);
  currentQuestionIndex = 0;
  renderQuestion();
  showPage('#page-quiz');
}
function renderQuestion() {
  const idx = currentQuestionIndex;
  const q = QUESTIONS[idx];
  $('#qIndex').textContent = '第 ' + (idx+1) + ' / ' + QUESTIONS.length + ' 题';
  $('#qText').textContent = q.q;
  $('#progressFill').style.width = (idx / QUESTIONS.length * 100) + '%';
  const wrap = $('#optionsWrap'); wrap.innerHTML = '';
  q.options.forEach((opt, i)=>{
    const btn = document.createElement('button');
    btn.className = 'option' + (currentAnswers[idx]===i ? ' selected' : '');
    btn.innerHTML = '<span class="tag">' + String.fromCharCode(65+i) + '</span>' + opt;
    btn.onclick = ()=>{ currentAnswers[idx]=i; renderQuestion(); };
    wrap.appendChild(btn);
  });
  $('#btnPrev').style.visibility = idx===0 ? 'hidden' : 'visible';
  $('#btnNext').textContent = idx===QUESTIONS.length-1 ? '查看结果' : '下一题';
}
$('#btnPrev').onclick = ()=>{ if(currentQuestionIndex>0){ currentQuestionIndex--; renderQuestion(); } };
$('#btnNext').onclick = ()=>{
  const idx = currentQuestionIndex;
  if (currentAnswers[idx]===-1) { showToast('请先选择一个选项'); return; }
  if (idx < QUESTIONS.length-1) { currentQuestionIndex++; renderQuestion(); }
  else { calculateResult(); }
};

function calculateResult() {
  const scores = { lin:0, sun:0, xue:0, wang:0, fu:0, guan:0, fang:0, kuang:0 };
  currentAnswers.forEach((ans,i)=>{
    if (ans===0) { scores.lin+=1; scores.fu+=0.5; scores.fang+=0.5; }
    if (ans===1) { scores.xue+=1; scores.wang+=0.5; scores.guan+=0.5; }
    if (ans===2) { scores.sun+=1; scores.kuang+=0.5; }
  });
  const a0 = currentAnswers[0], a1 = currentAnswers[1];
  if (a0===2||a1===2) scores.sun+=2;
  if (a0===1||a1===1) scores.xue+=2;
  if (a0===0) scores.lin+=2;
  if (currentAnswers[3]===1||currentAnswers[4]===1) scores.wang+=2;
  if (currentAnswers[5]===0) scores.fu+=2;
  if (currentAnswers[6]===0||currentAnswers[7]===0) scores.lin+=2;
  if (currentAnswers[9]===0) scores.fang+=2;
  if (currentAnswers[11]===2) scores.kuang+=2;
  let maxKey='lin', maxVal=-1;
  Object.keys(scores).forEach(k=>{ if(scores[k]>maxVal){ maxVal=scores[k]; maxKey=k; } });
  showResult(maxKey);
}

function showResult(key) {
  const r = RESULTS[key];
  $('#resultName').textContent = r.name + ' · ' + r.title;
  $('#resultSub').textContent = r.sub + ' —— ' + r.source;
  $('#resultKeyword').textContent = r.keyword;
  $('#resultAnalysis').textContent = r.analysis;
  $('#resultAdvice').textContent = r.advice;
  $('#resultTags').innerHTML = r.tags.map(t=>'<span>'+t+'</span>').join('');
  drawRadar(r.radar);
  if (supabaseClient) { supabaseClient.from('results').insert({ code:currentCode, result_key:key, answers:currentAnswers }).then(()=>{}); }
  showPage('#page-result');
  $('#btnSaveImg').onclick = ()=>saveLongImage(key);
  $('#btnCopyLink').onclick = ()=>copyShareLink(key);
  $('#btnRetake').onclick = ()=>{ startQuiz(); };
  $('#btnReview').onclick = ()=>showModal('#modalReview');
}

function drawRadar(values) {
  const canvas = $('#radarCanvas'); const ctx = canvas.getContext('2d');
  const w=canvas.width, h=canvas.height, cx=w/2, cy=h/2, R=80;
  const labels = ['内耗','外化','面子','关系','孤独'];
  ctx.clearRect(0,0,w,h);
  ctx.strokeStyle='#3d3020'; ctx.lineWidth=1;
  for(let ring=1; ring<=4; ring++){
    ctx.beginPath();
    for(let i=0;i<=5;i++){
      const angle = -Math.PI/2 + i*(Math.PI*2/5);
      const r = R*ring/4;
      const x = cx+Math.cos(angle)*r, y = cy+Math.sin(angle)*r;
      if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
    }
    ctx.closePath(); ctx.stroke();
  }
  for(let i=0;i<5;i++){
    const angle = -Math.PI/2 + i*(Math.PI*2/5);
    ctx.beginPath(); ctx.moveTo(cx,cy);
    ctx.lineTo(cx+Math.cos(angle)*R, cy+Math.sin(angle)*R); ctx.stroke();
  }
  ctx.beginPath();
  for(let i=0;i<=5;i++){
    const idx = i%5;
    const angle = -Math.PI/2 + idx*(Math.PI*2/5);
    const r = R*(values[idx]||50)/100;
    const x = cx+Math.cos(angle)*r, y = cy+Math.sin(angle)*r;
    if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
  }
  ctx.closePath();
  ctx.fillStyle='rgba(212,175,55,0.25)'; ctx.fill();
  ctx.strokeStyle='#d4af37'; ctx.lineWidth=2; ctx.stroke();
  ctx.fillStyle='#b8a88a'; ctx.font='11px sans-serif'; ctx.textAlign='center';
  for(let i=0;i<5;i++){
    const angle = -Math.PI/2 + i*(Math.PI*2/5);
    const x = cx+Math.cos(angle)*(R+18);
    const y = cy+Math.sin(angle)*(R+18)+4;
    ctx.fillText(labels[i], x, y);
  }
}

function saveLongImage(key) {
  const r = RESULTS[key];
  const cap = $('#capture-area');
  cap.innerHTML =
    '<div class="cap-title">你在人情社会里，是哪一种“名场面”主角？</div>' +
    '<div class="cap-sub">12道中国式真实情境 · 测出你骨子里的处世剧本</div>' +
    '<div class="cap-name">' + r.name + '</div>' +
    '<div class="cap-role">' + r.title + ' · ' + r.source + '</div>' +
    '<div class="cap-tags">' + r.tags.map(t=>'<span>'+t+'</span>').join('') + '</div>' +
    '<div class="cap-section"><h4>📖 命运关键词</h4><p>' + r.keyword + '</p></div>' +
    '<div class="cap-section"><h4>🔍 深度解析</h4><p>' + r.analysis + '</p></div>' +
    '<div class="cap-section"><h4>💡 改命建议</h4><p>' + r.advice + '</p></div>' +
    '<div class="cap-footer">扫码或打开链接，测测你是哪一种名场面主角</div>';
  showToast('正在生成图片...');
  html2canvas(cap, { backgroundColor:'#1a1410', scale:2, useCORS:true }).then(canvas=>{
    const link = document.createElement('a');
    link.download = '名场面测评_' + r.name + '.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
    showToast('图片已保存，请到相册/下载查看');
  }).catch(()=>showToast('生成失败，请重试'));
}

function copyShareLink(key) {
  const url = location.origin + location.pathname + '?r=' + key;
  if (navigator.clipboard) {
    navigator.clipboard.writeText(url).then(()=>showToast('链接已复制，快去分享吧'));
  } else {
    const ta = document.createElement('textarea'); ta.value=url; document.body.appendChild(ta); ta.select(); document.execCommand('copy'); document.body.removeChild(ta);
    showToast('链接已复制，快去分享吧');
  }
}

$('#btnCloseReview').onclick = ()=>{ $('#modalReview').classList.remove('show'); };

(function initFromUrl(){
  const params = new URLSearchParams(location.search);
  const r = params.get('r');
  if (r && RESULTS[r]) showResult(r);
})();
</script>
</body>
</html>
