<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Badges4readme Catalogue</title>
<style>
:root {color-scheme:dark;--bg:#0d1117;--panel:#161b22;--line:#30363d;--text:#e6edf3;--muted:#8b949e;--accent:#58a6ff}
* {box-sizing:border-box}
body {margin:0;background:var(--bg);color:var(--text);font:14px/1.4 system-ui,-apple-system,"Segoe UI",sans-serif}
header {position:sticky;top:0;z-index:2;background:var(--bg);border-bottom:1px solid var(--line);padding:10px 16px}
.top {max-width:1400px;margin:auto;display:flex;align-items:center;gap:7px;flex-wrap:wrap}
.top input[type=search] {flex:1;min-width:180px;background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:7px 9px;font:inherit;outline:none}
.top input:focus {border-color:var(--accent)}
button {font:inherit;cursor:pointer}
.top button {background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:6px 9px;white-space:nowrap}
.top select,.top input[type=color] {background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:5px 7px;font:inherit}
.top select {max-width:150px}
.color-control {display:flex;align-items:center;gap:4px;color:var(--muted);font-size:11px;white-space:nowrap}
.top input[type=color] {width:30px;height:30px;padding:2px;cursor:pointer}
.top button {background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:6px 9px;white-space:nowrap}
main {max-width:1400px;margin:auto;padding:10px 12px 24px}
footer {max-width:1400px;margin:18px auto 0;padding:13px 16px 16px;border-top:1px solid var(--line);color:var(--muted);font-size:12px;text-align:center;letter-spacing:.01em}
footer a {color:var(--text);text-decoration:none}
footer a:hover {color:var(--accent);text-decoration:underline}
.status {font-size:12px;color:var(--muted);margin:0 0 10px}
.hint {margin:0 0 10px;text-align:center;color:var(--muted);font-size:12px}
.category {margin:0 0 14px;scroll-margin-top:60px}
.category h2 {font-size:14px;font-weight:650;margin:0 0 6px}
.items {display:flex;flex-wrap:wrap;gap:4px}
.alphabet {display:flex;flex-wrap:wrap;gap:5px;margin:0 0 14px}
.alphabet button {min-width:30px;padding:6px 9px;background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px}
.alphabet button:hover,.alphabet button:focus-visible,.alphabet button.active {border-color:var(--accent);color:var(--accent);outline:none}
.badge-preview {display:block;margin:12px 0}
.badge-preview img {max-width:100%;height:auto;vertical-align:middle}
.badge-result {display:flex;align-items:center;gap:10px;flex-wrap:wrap;padding:8px 0;border-bottom:1px solid var(--line)}
.badge-result button {background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:5px 8px}
.item {padding: 0; gap: 0;}
.item {display:flex;align-items:center;gap:6px;background:var(--panel);border:1px solid var(--line);border-radius:3px;color:var(--text);text-align:left}
.item:hover,.item:focus-visible {border-color:var(--muted);outline:none}
.item img {display:block;width:auto;height:28px;max-width:100%;object-fit:contain;flex:none}
.load-more {display:block;margin:12px auto;background:var(--panel);color:var(--text);border:1px solid var(--line);border-radius:3px;padding:7px 12px}
.item span {font-size:12px;overflow-wrap:anywhere}
.empty {color:var(--muted);padding:12px 0}
.toast {position:fixed;bottom:16px;left:50%;transform:translate(-50%,8px);opacity:0;pointer-events:none;background:#e6edf3;color:#0d1117;padding:7px 11px;border-radius:3px;font-size:12px;transition:opacity .12s;z-index:5}
.toast.show {opacity:1;transform:translate(-50%,0)}
@media (max-width:600px) {
    header {padding:7px}
    main {padding:9px 7px}
    .top button {padding:6px 8px}
    .items {gap:3px}
    .item {padding:4px 6px}
}
</style>
</head>
<body>
<noscript>
    <div style="padding:12px;background:#3b1e1e;color:#fff;text-align:center">
        JavaScript must be enabled to use this badge catalogue.
    </div>
</noscript>
<header>
    <div class="top">
        <input id="search" type="search" placeholder="Search badges…" autocomplete="off" aria-label="Search badges">
        <label class="color-control">Text <input id="textColor" type="color" value="#ffffff" aria-label="Text color"></label>
        <label class="color-control">Icon <input id="iconColor" type="color" value="#ffffff" aria-label="Icon color"></label>
        <label class="color-control">Background <input id="bgColor" type="color" value="#111111" aria-label="Background color"></label>
        <button id="resetColors" type="button" title="Restore the default text, icon, and background colors">Reset colors</button>
        <select id="copyFormat" aria-label="Copy as">
            <option value="url">Copy as: URL</option>
            <option value="markdown" selected>Copy as: Markdown</option>
            <option value="html">Copy as: HTML</option>
        </select>
    </div>
</header>
<main>
    <p class="status" id="status">Loading icons…</p>
    <div id="catalogue"></div>
</main>
<footer>
    by <a href="https://github.com/xsyanic" target="_blank" rel="noopener noreferrer">prathvii / @xsyanic</a>
    · <a href="https://github.com/xsyanic/badges4readme" target="_blank" rel="noopener noreferrer">source</a>
</footer>
<div class="toast" id="toast" role="status">Copied</div>
<script>
const SOURCE = 'https://raw.githubusercontent.com/simple-icons/simple-icons/refs/heads/master/slugs.md';
const CDN = 'https://cdn.simpleicons.org/';
const catalogue = document.querySelector('#catalogue');
const search = document.querySelector('#search');
const status = document.querySelector('#status');
const toast = document.querySelector('#toast');
const copyFormat = document.querySelector('#copyFormat');
const textColor = document.querySelector('#textColor');
const iconColor = document.querySelector('#iconColor');
const bgColor = document.querySelector('#bgColor');
const resetColors = document.querySelector('#resetColors');
const defaultColors = { text: '#ffffff', icon: '#ffffff', background: '#111111' };
let icons = [];
let toastTimer;
let filteredIcons = [];
let activeLetter = null;
let activeIcon = null;

function titleCase(slug){return slug.split('-').map(s=>s?s[0].toUpperCase()+s.slice(1):'').join(' ')}

function parseSlugs(md){
    const found=[];
    for(const line of md.split(/\r?\n/)){
        if(!line.trim().startsWith('|'))continue;
        const cols=line.split('|').slice(1,-1).map(x=>x.trim());
        if(cols.length<2||/^[-: ]+$/.test(cols[0])||/^[-: ]+$/.test(cols[1])||/^(name|title|slug)$/i.test(cols[0])||/^(name|title|slug)$/i.test(cols[1]))continue;
        const name=cols[0].replace(/`/g,'').trim(),slug=cols[1].replace(/`/g,'').trim();
        if(!/^[a-z0-9-]+$/.test(slug)||!name||name.length>100)continue;
        found.push({name,slug});
    }
    if(found.length)return [...new Map(found.map(x=>[x.slug,x])).values()];
    return md.split(/\r?\n/).map(s=>s.trim()).filter(s=>/^[a-z0-9-]+$/.test(s)).map(slug=>({slug,name:titleCase(slug)}));
}

function badgeUrl(icon){
    const name=icon.name.replace(/-/g,'--').replace(/_/g,'__');
    const bg=bgColor.value.slice(1),ic=iconColor.value.slice(1),tc=textColor.value.slice(1);
    return `https://img.shields.io/badge/${encodeURIComponent(name)}-${tc}?style=for-the-badge&logo=${encodeURIComponent(icon.slug)}&logoColor=${ic}&labelColor=${bg}&color=${bg}`;
}

function badgeMarkup(icon,format){
    const url=badgeUrl(icon);
    const alt=icon.name.replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    if(format==='url')return url;
    if(format==='markdown')return `![${icon.name}](${url})`;
    return `<img alt="${alt}" src="${url}">`;
}

function render(){
    const q=search.value.trim().toLowerCase();
    filteredIcons=icons.filter(x=>x.name.toLowerCase().includes(q)||x.slug.includes(q));
    catalogue.replaceChildren();
    activeIcon=null;

    if(q){
        status.textContent=`${filteredIcons.length.toLocaleString()} badges matching search · click a result to copy`;
        const results=document.createElement('div');
        for(const icon of filteredIcons.slice(0,100)){
            const row=document.createElement('div');row.className='badge-result';
            const label=document.createElement('span');label.textContent=icon.name;
            const preview=document.createElement('img');preview.loading='lazy';preview.decoding='async';preview.alt='';preview.height=28;preview.src=badgeUrl(icon);
            const copy=document.createElement('button');copy.type='button';copy.textContent='Copy';copy.addEventListener('click',()=>copyText(badgeMarkup(icon,copyFormat.value)));
            const show=document.createElement('button');show.type='button';show.textContent='Preview';show.addEventListener('click',()=>showPreview(icon));
            row.append(label,preview,show,copy);results.append(row);
        }
        catalogue.append(results);
        if(!filteredIcons.length){const empty=document.createElement('div');empty.className='empty';empty.textContent='No badges found.';catalogue.append(empty)}
        if(filteredIcons.length>100){const note=document.createElement('p');note.className='hint';note.textContent='Showing first 100 matches. Refine your search to narrow the results.';catalogue.append(note)}
        return;
    }

    status.textContent='Choose a character to browse badges. Nothing loads until you select one.';
    const alphabet=document.createElement('nav');alphabet.className='alphabet';alphabet.setAttribute('aria-label','Browse badges by first character');
    const characters=['.','#',...'ABCDEFGHIJKLMNOPQRSTUVWXYZ'];
    for(const character of characters){
        const button=document.createElement('button');button.type='button';button.textContent=character;
        button.classList.toggle('active',activeLetter===character);
        button.addEventListener('click',()=>{activeLetter=character;render();renderLetter(character)});
        alphabet.append(button);
    }
    catalogue.append(alphabet);
    if(activeLetter)renderLetter(activeLetter);
}

function renderLetter(letter){
    let section=catalogue.querySelector('.letter-results');
    if(section)section.remove();
    section=document.createElement('section');section.className='letter-results';
    const heading=document.createElement('h2');heading.className='category';heading.textContent=`${letter} badges`;
    section.append(heading);
    const matches=icons.filter(icon=>{
        const first=(icon.name[0]||'#').toUpperCase();
        return letter==='.' ? !/[A-Z0-9]/i.test(first) : letter==='#' ? /^[0-9]/.test(first) : first===letter;
    });
    const list=document.createElement('div');list.className='items';
    for(const icon of matches){
        const button=document.createElement('button');button.type='button';button.className='item';button.textContent=icon.name;
        button.addEventListener('click',()=>showPreview(icon));
        list.append(button);
    }
    if(!matches.length){const empty=document.createElement('div');empty.className='empty';empty.textContent='No badges in this group.';section.append(empty)}
    else section.append(list);
    catalogue.append(section);
    status.textContent=`${matches.length.toLocaleString()} badges under “${letter}” · select a name to preview and copy`;
}

function showPreview(icon){
    activeIcon=icon;
    let preview=catalogue.querySelector('.badge-preview');
    if(!preview){preview=document.createElement('section');preview.className='badge-preview';catalogue.prepend(preview)}
    preview.replaceChildren();
    const heading=document.createElement('h2');heading.textContent=icon.name;
    const img=document.createElement('img');img.loading='lazy';img.decoding='async';img.alt=`${icon.name} badge`;img.src=badgeUrl(icon);
    const copy=document.createElement('button');copy.type='button';copy.textContent=`Copy as ${copyFormat.value}`;copy.addEventListener('click',()=>copyText(badgeMarkup(icon,copyFormat.value)));
    preview.append(heading,img,copy);
    preview.scrollIntoView({behavior:'smooth',block:'nearest'});
}

async function copyText(value){
    try{await navigator.clipboard.writeText(value)}
    catch{
        const ta=document.createElement('textarea');ta.value=value;ta.style.position='fixed';ta.style.opacity='0';document.body.append(ta);ta.select();
        const ok=document.execCommand('copy');ta.remove();if(!ok){window.prompt('Copy badge:',value);return}
    }
    toast.textContent='Copied to clipboard';toast.classList.add('show');clearTimeout(toastTimer);toastTimer=setTimeout(()=>toast.classList.remove('show'),1100);
}

document.addEventListener('keydown',event=>{
    if(event.defaultPrevented||event.ctrlKey||event.metaKey||event.altKey)return;
    const target=event.target;
    if(target instanceof HTMLElement&&(target.isContentEditable||/^(INPUT|TEXTAREA|SELECT|BUTTON)$/.test(target.tagName)))return;
    if(event.key.length===1&&!event.key.match(/\s/)||event.key===' '){
        event.preventDefault();search.focus();if(event.key!==' ')search.value+=event.key;activeLetter=null;render();
    }else if(event.key==='Escape'&&document.activeElement===search){search.value='';activeLetter=null;render()}
});
search.addEventListener('input',()=>{activeLetter=null;render()});
for(const picker of [textColor,iconColor,bgColor])picker.addEventListener('input',()=>{if(activeIcon)showPreview(activeIcon)});
resetColors.addEventListener('click',()=>{textColor.value=defaultColors.text;iconColor.value=defaultColors.icon;bgColor.value=defaultColors.background;if(activeIcon)showPreview(activeIcon)});
copyFormat.addEventListener('change',()=>{const button=catalogue.querySelector('.badge-preview button');if(button)button.textContent=`Copy as ${copyFormat.value}`});

(async()=>{
    try{
        const response=await fetch(SOURCE,{cache:'no-cache'});
        if(!response.ok)throw new Error(`HTTP ${response.status}`);
        icons=parseSlugs(await response.text()).sort((a,b)=>a.name.localeCompare(b.name));
        if(!icons.length)throw new Error('No slugs parsed');
        render();
    }catch(error){
        status.textContent='Could not load slugs.md. Try serving this file over localhost.';
        const link=document.createElement('a');link.href='https://github.com/simple-icons/simple-icons/blob/master/slugs.md';link.textContent='slugs.md';link.style.color='var(--accent)';catalogue.append(link);console.error(error)
    }
})();
</script>
</body>
</html>
