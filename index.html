/**
 * レシート家計簿システム — server.js
 * 起動: node server.js
 * 環境変数: PORT=3000  DB_PATH=./kakeibo.db
 */

'use strict';
require('dotenv').config();

const express   = require('express');
const cors      = require('cors');
const fs        = require('fs');
const path      = require('path');
const initSqlJs = require('sql.js');

// ─── 設定 ────────────────────────────────────────────────
const PORT    = process.env.PORT    || 3000;
const DB_PATH = process.env.DB_PATH || './kakeibo.db';

// ─── DB ──────────────────────────────────────────────────
let _db = null;

async function getDb() {
  if (_db) return _db;
  const SQL = await initSqlJs();
  _db = fs.existsSync(DB_PATH)
    ? new SQL.Database(fs.readFileSync(DB_PATH))
    : new SQL.Database();
  _db.run('PRAGMA foreign_keys = ON;');
  _db.run(`
    CREATE TABLE IF NOT EXISTS receipts (
      id           INTEGER PRIMARY KEY AUTOINCREMENT,
      date         TEXT    NOT NULL,
      store        TEXT    NOT NULL,
      total_amount INTEGER NOT NULL,
      created_at   TEXT    DEFAULT (datetime('now','localtime'))
    );
    CREATE TABLE IF NOT EXISTS receipt_items (
      id         INTEGER PRIMARY KEY AUTOINCREMENT,
      receipt_id INTEGER NOT NULL,
      name       TEXT    NOT NULL,
      price      INTEGER NOT NULL,
      FOREIGN KEY (receipt_id) REFERENCES receipts(id) ON DELETE CASCADE
    );
    CREATE INDEX IF NOT EXISTS idx_receipts_date ON receipts(date);
  `);
  dbSave(_db);
  return _db;
}

function dbSave(db) {
  fs.writeFileSync(DB_PATH, Buffer.from(db.export()));
}

// ─── HTML / CSS / JS ─────────────────────────────────────
const HTML = /* html */`<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="theme-color" content="#0f0f12">
  <title>レシート家計簿</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500;700&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#0f0f12;--surface:#18181f;--surface2:#22222c;
      --border:#2e2e3d;--accent:#7ee8a2;--accent2:#38bdf8;
      --text:#e8e8f0;--muted:#7b7b94;--danger:#f87171;
      --r:16px;--r-sm:10px;
    }
    *{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
    html{font-size:16px;scroll-behavior:smooth}
    body{font-family:'Noto Sans JP',sans-serif;background:var(--bg);color:var(--text);
         min-height:100dvh;overflow-x:hidden}

    /* ── header ── */
    .hd{position:sticky;top:0;z-index:100;
        background:rgba(15,15,18,.85);backdrop-filter:blur(20px);
        -webkit-backdrop-filter:blur(20px);border-bottom:1px solid var(--border);
        padding:env(safe-area-inset-top,0px) 0 0}
    .hd-inner{display:flex;align-items:center;gap:10px;padding:14px 20px}
    .logo-icon{width:36px;height:36px;border-radius:10px;display:flex;
               align-items:center;justify-content:center;font-size:18px;flex-shrink:0;
               background:linear-gradient(135deg,var(--accent),var(--accent2))}
    .logo-text{font-size:17px;font-weight:700;letter-spacing:-.3px}
    .logo-sub{font-size:11px;color:var(--muted)}

    /* ── main ── */
    .main{padding:20px 16px calc(env(safe-area-inset-bottom,0px) + 24px);
          max-width:640px;margin:0 auto}

    /* ── summary card ── */
    .summary{background:linear-gradient(135deg,#1a2634,#1a1f2e);
             border:1px solid var(--border);border-radius:var(--r);
             padding:24px 20px;margin-bottom:20px;position:relative;overflow:hidden}
    .summary::before{content:'';position:absolute;top:-40px;right:-40px;
                     width:120px;height:120px;pointer-events:none;
                     background:radial-gradient(circle,rgba(126,232,162,.15),transparent 70%)}
    .month-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px}
    .nav-btn{background:var(--surface2);border:1px solid var(--border);color:var(--text);
             width:36px;height:36px;border-radius:50%;font-size:16px;cursor:pointer;
             display:flex;align-items:center;justify-content:center;transition:background .15s}
    .nav-btn:active{background:var(--border)}
    .cur-month{font-size:15px;font-weight:500;color:var(--muted)}
    .sum-label{font-size:12px;color:var(--muted);letter-spacing:.5px}
    .sum-amount{font-family:'DM Mono',monospace;font-size:40px;font-weight:500;
                color:var(--accent);letter-spacing:-1px;line-height:1;margin-bottom:6px}
    .stats{display:flex;gap:16px;margin-top:16px;padding-top:16px;border-top:1px solid var(--border)}
    .stat-val{font-family:'DM Mono',monospace;font-size:20px;font-weight:500;color:var(--text)}
    .stat-lbl{font-size:11px;color:var(--muted);margin-top:2px}

    /* ── section header ── */
    .sec-hd{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px}
    .sec-title{font-size:13px;font-weight:600;color:var(--muted);letter-spacing:.8px;text-transform:uppercase}
    .sec-count{font-size:12px;color:var(--muted);font-family:'DM Mono',monospace}

    /* ── receipt list ── */
    .list{display:flex;flex-direction:column;gap:10px}
    .card{background:var(--surface);border:1px solid var(--border);
          border-radius:var(--r);overflow:hidden;transition:border-color .15s;
          animation:fadeIn .3s ease both}
    @keyframes fadeIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
    .card:active{border-color:var(--accent2)}
    .card-hd{display:flex;align-items:center;justify-content:space-between;
             padding:14px 16px;cursor:pointer;user-select:none}
    .store-name{font-size:16px;font-weight:600;margin-bottom:4px;
                display:flex;align-items:center;gap:8px}
    .card-date{font-size:12px;color:var(--muted);font-family:'DM Mono',monospace}
    .card-amt{text-align:right;flex-shrink:0}
    .amt-val{font-family:'DM Mono',monospace;font-size:20px;font-weight:500;color:var(--accent)}
    .amt-yen{font-size:13px;color:var(--muted);margin-right:2px}
    .exp-icon{font-size:12px;color:var(--muted);margin-left:8px;
              transition:transform .2s;display:inline-block}
    .card.open .exp-icon{transform:rotate(180deg)}

    /* ── items ── */
    .items{display:none;border-top:1px solid var(--border)}
    .card.open .items{display:block}
    .item-row{display:flex;align-items:center;justify-content:space-between;
              padding:10px 16px;border-bottom:1px solid rgba(46,46,61,.5);gap:12px}
    .item-row:last-child{border-bottom:none}
    .item-name{font-size:13px;flex:1}
    .item-price{font-family:'DM Mono',monospace;font-size:13px;color:var(--muted);flex-shrink:0}
    .del-btn{background:none;border:none;color:var(--danger);font-size:12px;cursor:pointer;
             padding:4px 8px;border-radius:var(--r-sm);opacity:.6;transition:opacity .15s}
    .del-btn:active{opacity:1;background:rgba(248,113,113,.1)}
    .items-foot{display:flex;justify-content:flex-end;
                padding:10px 16px;border-top:1px solid var(--border)}

    /* ── empty / loading ── */
    .empty{text-align:center;padding:60px 20px;color:var(--muted)}
    .empty-icon{font-size:48px;margin-bottom:16px;opacity:.5}
    .empty-title{font-size:16px;font-weight:600;margin-bottom:8px;color:var(--text)}
    .empty-desc{font-size:13px;line-height:1.6}
    .loading{text-align:center;padding:40px;color:var(--muted);font-size:14px}
    .spinner{width:24px;height:24px;border:2px solid var(--border);border-top-color:var(--accent);
             border-radius:50%;animation:spin .8s linear infinite;margin:0 auto 12px}
    @keyframes spin{to{transform:rotate(360deg)}}

    /* ── toast ── */
    .toast{position:fixed;bottom:calc(env(safe-area-inset-bottom,0px) + 24px);left:50%;
           transform:translateX(-50%) translateY(100px);background:var(--surface2);
           border:1px solid var(--border);border-radius:100px;padding:12px 20px;
           font-size:14px;font-weight:500;white-space:nowrap;z-index:999;
           transition:transform .3s cubic-bezier(.34,1.56,.64,1);
           box-shadow:0 4px 24px rgba(0,0,0,.4)}
    .toast.show{transform:translateX(-50%) translateY(0)}
    .toast.success{color:var(--accent)}
    .toast.error{color:var(--danger)}

    @media(min-width:640px){
      .main{padding:28px 24px}
      .sum-amount{font-size:52px}
      .list{gap:12px}
    }
  </style>
</head>
<body>
  <header class="hd">
    <div class="hd-inner">
      <div class="logo-icon">🧾</div>
      <div>
        <div class="logo-text">レシート家計簿</div>
        <div class="logo-sub">iPhone ショートカット連携</div>
      </div>
    </div>
  </header>

  <main class="main">
    <div class="summary">
      <div class="month-nav">
        <button class="nav-btn" id="prevMonth">‹</button>
        <span class="cur-month" id="curMonth"></span>
        <button class="nav-btn" id="nextMonth">›</button>
      </div>
      <div class="sum-label">今月の合計支出</div>
      <div class="sum-amount">¥<span id="totalAmount">---</span></div>
      <div class="stats">
        <div><div class="stat-val" id="cntStat">-</div><div class="stat-lbl">レシート枚数</div></div>
        <div><div class="stat-val">¥<span id="avgStat">-</span></div><div class="stat-lbl">平均購入額</div></div>
      </div>
    </div>

    <div class="sec-hd">
      <span class="sec-title">レシート一覧</span>
      <span class="sec-count" id="secCount">-</span>
    </div>
    <div class="list" id="list">
      <div class="loading"><div class="spinner"></div>読み込み中…</div>
    </div>
  </main>

  <script>
    let Y = new Date().getFullYear(), M = new Date().getMonth() + 1;
    const $ = id => document.getElementById(id);
    const fmt = n => Number(n).toLocaleString('ja-JP');

    document.addEventListener('DOMContentLoaded', () => {
      render(); load();
      $('prevMonth').onclick = () => { if(--M<1){M=12;Y--}; render(); load(); };
      $('nextMonth').onclick = () => {
        const n=new Date(); if(Y===n.getFullYear()&&M===n.getMonth()+1)return;
        if(++M>12){M=1;Y++}; render(); load();
      };
    });

    function render(){ $('curMonth').textContent = Y+'年'+M+'月'; }

    async function load() {
      $('list').innerHTML = '<div class="loading"><div class="spinner"></div>読み込み中…</div>';
      $('totalAmount').textContent = '---';
      try {
        const d = await fetch('/api/receipts?year='+Y+'&month='+M).then(r=>r.json());
        const rs = d.receipts || [];
        const total = rs.reduce((s,r)=>s+r.total_amount, 0);
        const cnt   = rs.length;
        $('totalAmount').textContent = fmt(total);
        $('cntStat').textContent = cnt;
        $('avgStat').textContent  = fmt(cnt ? Math.round(total/cnt) : 0);
        $('secCount').textContent = cnt+' 件';

        if (!cnt) {
          $('list').innerHTML = '<div class="empty"><div class="empty-icon">🧾</div>'
            +'<div class="empty-title">レシートがありません</div>'
            +'<div class="empty-desc">iPhoneショートカットでレシートを<br>撮影して登録しましょう</div></div>';
          return;
        }
        $('list').innerHTML = rs.map((r,i) => {
          const itemsHtml = r.items.length
            ? r.items.map(it=>'<div class="item-row"><span class="item-name">'+esc(it.name)+'</span>'
                +'<span class="item-price">¥'+fmt(it.price)+'</span></div>').join('')
            : '<div class="item-row"><span class="item-name" style="color:var(--muted)">品目情報なし</span></div>';
          return '<div class="card" id="c'+r.id+'" style="animation-delay:'+i*.04+'s">'
            +'<div class="card-hd" onclick="tog('+r.id+')">'
            +'<div><div class="store-name">🏪 '+esc(r.store)
            +'<span class="exp-icon" id="ei'+r.id+'">▼</span></div>'
            +'<div class="card-date">'+r.date+'</div></div>'
            +'<div class="card-amt"><span class="amt-yen">¥</span>'
            +'<span class="amt-val">'+fmt(r.total_amount)+'</span></div></div>'
            +'<div class="items" id="it'+r.id+'">'+itemsHtml
            +'<div class="items-foot"><button class="del-btn" onclick="del('+r.id+')">🗑 削除</button></div>'
            +'</div></div>';
        }).join('');
      } catch(e) { toast('データの取得に失敗しました','error'); }
    }

    function tog(id) { $('c'+id).classList.toggle('open'); }

    async function del(id) {
      if (!confirm('このレシートを削除しますか？')) return;
      const r = await fetch('/api/receipt/'+id,{method:'DELETE'});
      toast(r.ok ? '削除しました' : '削除に失敗しました', r.ok ? 'success' : 'error');
      if (r.ok) load();
    }

    function toast(msg, type='success') {
      let t = document.querySelector('.toast');
      if (!t) { t=document.createElement('div'); t.className='toast'; document.body.appendChild(t); }
      t.textContent = msg; t.className = 'toast '+type;
      requestAnimationFrame(()=>requestAnimationFrame(()=>t.classList.add('show')));
      setTimeout(()=>t.classList.remove('show'), 2600);
    }

    function esc(s) {
      return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;')
                      .replace(/>/g,'&gt;').replace(/"/g,'&quot;');
    }
  </script>
</body>
</html>`;

// ─── Express ─────────────────────────────────────────────
const app = express();
app.use(cors());
app.use(express.json());

// GET / — フロントエンド
app.get('/', (_req, res) => res.send(HTML));

// POST /api/receipt — レシート登録
app.post('/api/receipt', async (req, res) => {
  const { date, store, total_amount, items } = req.body;
  if (!date || !store || total_amount == null)
    return res.status(400).json({ error: 'date, store, total_amount は必須です' });
  if (!/^\d{4}-\d{2}-\d{2}$/.test(date))
    return res.status(400).json({ error: 'date は YYYY-MM-DD 形式で入力してください' });
  try {
    const db = await getDb();
    db.run('INSERT INTO receipts (date,store,total_amount) VALUES (?,?,?)',
           [date, store, Math.round(total_amount)]);
    const id = db.exec('SELECT last_insert_rowid() AS id')[0].values[0][0];
    if (Array.isArray(items)) {
      for (const it of items)
        if (it.name && it.price != null)
          db.run('INSERT INTO receipt_items (receipt_id,name,price) VALUES (?,?,?)',
                 [id, it.name, Math.round(it.price)]);
    }
    dbSave(db);
    res.status(201).json({ success: true, message: 'レシートを保存しました', receipt_id: id });
  } catch (e) {
    console.error(e);
    res.status(500).json({ error: 'データベースへの保存に失敗しました' });
  }
});

// GET /api/receipts — 一覧取得
app.get('/api/receipts', async (req, res) => {
  const { year, month } = req.query;
  try {
    const db = await getDb();
    let sql = 'SELECT id,date,store,total_amount,created_at FROM receipts';
    const p = [];
    if (year && month) {
      sql += " WHERE strftime('%Y',date)=? AND strftime('%m',date)=?";
      p.push(year, String(month).padStart(2,'0'));
    }
    sql += ' ORDER BY date DESC, created_at DESC';
    const result = db.exec(sql, p);
    const rows = result.length
      ? result[0].values.map(v => ({ id:v[0], date:v[1], store:v[2], total_amount:v[3], created_at:v[4] }))
      : [];
    const receipts = rows.map(r => {
      const ir = db.exec('SELECT name,price FROM receipt_items WHERE receipt_id=?', [r.id]);
      return { ...r, items: ir.length ? ir[0].values.map(v=>({name:v[0],price:v[1]})) : [] };
    });
    res.json({ receipts });
  } catch (e) {
    console.error(e);
    res.status(500).json({ error: '取得に失敗しました' });
  }
});

// GET /api/summary — 月次サマリー
app.get('/api/summary', async (_req, res) => {
  try {
    const db = await getDb();
    const r = db.exec(`SELECT strftime('%Y-%m',date) m, SUM(total_amount) t, COUNT(*) c
                       FROM receipts GROUP BY m ORDER BY m DESC LIMIT 12`);
    const monthly = r.length ? r[0].values.map(v=>({month:v[0],total:v[1],count:v[2]})) : [];
    res.json({ monthly });
  } catch (e) {
    res.status(500).json({ error: '取得に失敗しました' });
  }
});

// DELETE /api/receipt/:id — レシート削除
app.delete('/api/receipt/:id', async (req, res) => {
  try {
    const db = await getDb();
    db.run('DELETE FROM receipts WHERE id=?', [req.params.id]);
    dbSave(db);
    res.json({ success: true });
  } catch (e) {
    res.status(500).json({ error: '削除に失敗しました' });
  }
});

// ─── 起動 ────────────────────────────────────────────────
app.listen(PORT, () => {
  console.log(`\n🧾 レシート家計簿`);
  console.log(`📡 http://localhost:${PORT}\n`);
});
