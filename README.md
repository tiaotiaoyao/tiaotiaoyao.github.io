import React, { useState, useEffect } from 'react';
import { Play, X, BookOpen, Mail, ArrowRight, ChevronRight, Instagram } from 'lucide-react';

// -----------------------------------------------------------------------------
// 數據源：基於 "姚易彤 TIAOTIAOYAO.pdf"
// -----------------------------------------------------------------------------

const CATEGORIES = [
  { id: 'all', label: '全部作品 ALL' },
  { id: 'film', label: '電影 FILM' },
  { id: 'dance', label: '舞蹈 DANCE' },
  { id: 'art', label: '文學與藝術 ART & LIT' },
];

// 注意：為了達到最佳效果，請將您 PDF 中的圖片截取後，替換以下的 'image' 連結
const WORKS = [
  {
    id: 1,
    type: 'film',
    title: '閒事兒',
    enTitle: 'Hansi',
    year: '2025',
    format: '紀錄片 Documentary | 90 min',
    tags: ['Family', 'Death', 'Karma'],
    // 建議使用 PDF 第 27 頁的海報
    image: 'https://placehold.co/600x850/1a1a1a/FFF?text=Hansi+Poster', 
    aspect: 'portrait', // 直式海報
    description: '這是一部關於家族與死亡的紀錄片電影，故事從導演姚易彤 93 歲奶奶的葬禮開始。影片有意捨棄了線性敘事與觀點表達，將鏡頭交還給生活本身。悲痛與幸福本為一體，是生活的常態。',
    enDescription: 'A documentary film about family and death, beginning at the funeral of the director\'s 93-year-old grandmother. It abandons linear narrative to return the lens to life itself, exploring the karmic ties of the paternal family.',
    linkData: { url: '#', password: 'Coming Soon' }
  },
  {
    id: 2,
    type: 'film',
    title: '觀照觀照',
    enTitle: 'Buddha Knows',
    year: '2023',
    format: '劇情/奇幻 Narrative/Fantasy | 29 min',
    tags: ['Fantasy', 'Spirituality', 'Award Winner'],
    // 建議使用 PDF 第 17 頁的海報
    image: 'https://placehold.co/600x850/222/FFF?text=Buddha+Knows+Poster',
    aspect: 'portrait',
    description: '口吃的小雪和殘疾的老陳已結婚五年，卻各處一屋。啞巴乞丐老二的出現打破了平靜。導演探索了空間關係與人物關係的深層對照，「觀照」意指內觀與照見。',
    enDescription: 'Xiaoxue, who stutters, and Lao Chen, who is disabled, live separate lives under one roof. The arrival of a mute beggar catalyzes a miraculous event. A story about inner observation and the healing of trauma through acceptance.',
    awards: ['2023 世界遊牧短片電影節十佳影片', '2024 獨立電影人獎最佳國際短片'],
    linkData: { url: 'https://example.com/watch/buddhaknows', password: 'love' }
  },
  {
    id: 3,
    type: 'film',
    title: '打撈空',
    enTitle: 'Small Talk',
    year: '2022',
    format: '劇情/奇幻 Narrative/Fantasy | 17 min',
    tags: ['Mockumentary', 'DV', 'Awakening'],
    // 建議使用 PDF 第 11 頁的海報
    image: 'https://placehold.co/600x850/111/FFF?text=Small+Talk+Poster',
    aspect: 'portrait',
    description: '一部用 DV 拍攝的偽紀錄片。玉玉總覺得有人跟蹤她，找來好朋友旦旦幫她拍攝取證。DV 成為了她們搜集證據、觀察世界的最佳「監視武器」，直到她們發現自己並非這個世界的人類。',
    enDescription: 'A mockumentary shot on DV. Yuyu feels stalked and asks her friend to film evidence. Through the lens, they uncover subtle clues leading to a sudden realization: they are not human beings of this world.',
    awards: ['2022 HI! SHORTS 廈門短片電影節主競賽單元', '2023 倫敦獨立短片電影節入圍'],
    linkData: { url: 'https://example.com/watch/smalltalk', password: 'love' }
  },
  {
    id: 4,
    type: 'film',
    title: '太快了',
    enTitle: 'Too Fast / A Brief',
    year: '2019',
    format: '劇情/奇幻 Narrative/Fantasy | 5 min',
    tags: ['Experimental', 'Time', 'Sound'],
    // 建議使用 PDF 第 22 頁的海報
    image: 'https://placehold.co/600x850/333/FFF?text=Too+Fast+Poster',
    aspect: 'portrait',
    description: '剪輯師小王將素材調節成 40% 的速度，隨之發生了意外，他的現實也變成了 40% 慢放的世界。影片探索了現實世界中隱藏的慢放聲音信息。',
    enDescription: 'Editor Xiao Wang slows his footage to 40% speed, only to find his reality slipping into a slow-motion world. An exploration of sound across different dimensions.',
    linkData: { url: 'https://example.com/watch/toofast', password: 'love' }
  },
  {
    id: 5,
    type: 'dance',
    title: '晨練',
    enTitle: 'Morning Practice',
    year: '2019',
    format: '舞蹈影像 Dance Film | 1 min',
    tags: ['Nature', 'Body', 'Yin Yang'],
    // 建議使用 PDF 第 30 頁的劇照
    image: 'https://placehold.co/800x450/2a2a2a/FFF?text=Morning+Practice+Still',
    aspect: 'landscape', // 橫式
    description: '靈感源於「須彌芥子」，象徵至大與至小的共存。舞者身著水木色調，置身山林，經驗水木相生的過程，陰陽合一，用身體與自然共振。',
    enDescription: 'Inspired by the concept of "Mount Sumeru in a mustard seed". Dancers TiaoTiao and Wen Xin, dressed in earth tones, resonate with nature in a forested landscape, embodying the cycle of water and wood.',
    linkData: { url: 'https://example.com/watch/morning', password: 'love' }
  },
  {
    id: 6,
    type: 'dance',
    title: '潮汐',
    enTitle: 'TIDE',
    year: '2016',
    format: '雙人舞 Duet | 5 min',
    tags: ['Flow', 'Relationship', 'Theater'],
    // 建議使用 PDF 第 32 頁的劇照
    image: 'https://placehold.co/800x450/1a1a1a/FFF?text=TIDE+Still',
    aspect: 'landscape',
    description: '靈感源於水中兩根藤條之間的流動關係。舞者演繹藤條在潮汐中時而纏繞、時而分離的狀態，堅硬又綿軟，同步又獨立。',
    enDescription: 'Inspired by two vines drifting in water. The duet explores the shifting connection between two bodies—firm yet soft, synchronized yet independent.',
    linkData: { url: 'https://example.com/watch/tide', password: 'love' }
  },
  {
    id: 7,
    type: 'art',
    title: '壩河記',
    enTitle: 'Ba River',
    year: '2024',
    format: '散文集 Essay Collection',
    tags: ['Miniature Book', 'Writing', 'Life'],
    // 建議使用 PDF 第 37 頁的書籍圖片
    image: 'https://placehold.co/600x600/444/FFF?text=Ba+River+Book',
    aspect: 'square', // 方形
    description: '微縮模型書，以廢紙製成。收錄了作者於 2024 年居住在北京東壩壩河期間的創作，以及《打撈空》和《觀照觀照》的幕後拍攝故事。',
    enDescription: 'A miniature book made from scrap paper. A collection of essays written while living by the Ba River, including behind-the-scenes stories of her films.'
  },
  {
    id: 8,
    type: 'art',
    title: '云母星飛船',
    enTitle: 'Mica Star Spaceship',
    year: '2025',
    format: '繪畫系列 Painting Series',
    tags: ['Sci-Fi', 'AI', 'Mixed Media'],
    // 建議使用 PDF 第 38/39 頁的繪畫
    image: 'https://placehold.co/800x600/222/FFF?text=Mica+Star+Art',
    aspect: 'landscape',
    description: '包含「斐波那契飛船」、「瓢蟲飛船」、「漢字造字飛船」。以彩鉛、馬克筆、丙烯顏料創作，結合 AI 動態展示。',
    enDescription: 'A series including "Fibonacci Spaceship", "Ladybug Spaceship", and "Kanji Spaceship". Created with mixed media and AI animation templates.'
  },
  {
    id: 9,
    type: 'art',
    title: '我的詩集一',
    enTitle: 'My Poetry I',
    year: '2010',
    format: '詩集 Poetry Collection',
    tags: ['Cosmos', 'Consciousness', 'Origin'],
    // 建議使用 PDF 第 34/35 頁的圖片
    image: 'https://placehold.co/600x800/111/FFF?text=Poetry+Manuscript',
    aspect: 'portrait',
    description: '收錄 29 首創作於 2009-2010 年的作品。多數詩篇圍繞宇宙、星球及多維意識展開。其中很多信息來自於第七維度云母星，是關於生命本源的說明書。',
    enDescription: '29 poems written between 2009-2010. Blending classical and contemporary styles, exploring the cosmos and multidimensional consciousness. Messages from Mica Star in the seventh dimension.'
  }
];

// -----------------------------------------------------------------------------
// 組件
// -----------------------------------------------------------------------------

const Header = ({ activeTab, setActiveTab, setView }) => (
  <header className="fixed top-0 left-0 w-full z-50 bg-black/40 backdrop-blur-md border-b border-white/10 transition-all duration-500">
    <div className="max-w-[1600px] mx-auto px-6 h-24 flex items-center justify-between">
      <div 
        className="cursor-pointer group relative z-10"
        onClick={() => { setView('home'); setActiveTab('all'); }}
      >
        <h1 className="text-2xl md:text-3xl font-serif tracking-[0.2em] text-white group-hover:text-neutral-300 transition-colors duration-300 drop-shadow-lg">
          TIAOTIAO YAO
        </h1>
        <div className="h-[1px] w-0 group-hover:w-full bg-white transition-all duration-500 ease-out mt-1"></div>
      </div>
      
      <nav className="hidden md:flex items-center space-x-12 text-xs tracking-[0.25em] font-medium text-neutral-300">
        <button 
          onClick={() => setView('about')}
          className={`hover:text-white transition-colors uppercase relative py-2 group drop-shadow-md ${activeTab === 'about' ? 'text-white' : ''}`}
        >
          About
          <span className={`absolute bottom-0 left-0 w-full h-[1px] bg-white transform scale-x-0 group-hover:scale-x-100 transition-transform duration-300 ${activeTab === 'about' ? 'scale-x-100' : ''}`}></span>
        </button>
        <button 
          onClick={() => { setView('home'); setActiveTab('all'); }}
           className={`hover:text-white transition-colors uppercase relative py-2 group drop-shadow-md ${activeTab !== 'about' ? 'text-white' : ''}`}
        >
          Works
          <span className={`absolute bottom-0 left-0 w-full h-[1px] bg-white transform scale-x-0 group-hover:scale-x-100 transition-transform duration-300 ${activeTab !== 'about' ? 'scale-x-100' : ''}`}></span>
        </button>
        <a href="mailto:tiaotiaofilm@qq.com" className="hover:text-white transition-colors uppercase flex items-center gap-2 drop-shadow-md">
          Contact <Mail size={12}/>
        </a>
      </nav>

      <button className="md:hidden text-white p-2" onClick={() => setView(view === 'home' ? 'about' : 'home')}>
        <div className="space-y-1.5 drop-shadow-md">
            <div className="w-6 h-px bg-white"></div>
            <div className="w-6 h-px bg-white"></div>
            <div className="w-6 h-px bg-white"></div>
        </div>
      </button>
    </div>
  </header>
);

const WorkCard = ({ work, onClick }) => {
  const aspectClass = {
    'portrait': 'aspect-[2/3]',
    'landscape': 'aspect-[16/9]',
    'square': 'aspect-square'
  }[work.aspect || 'portrait'];

  return (
    <div 
      className="group cursor-pointer flex flex-col gap-4 mb-12"
      onClick={() => onClick(work)}
    >
      <div className={`relative ${aspectClass} overflow-hidden bg-black/40 border border-white/10 transition-all duration-700 group-hover:border-white/30 backdrop-blur-sm shadow-2xl`}>
        <img 
          src={work.image} 
          alt={work.title}
          className="w-full h-full object-cover transition-transform duration-1000 ease-out group-hover:scale-105 opacity-90 group-hover:opacity-100 grayscale group-hover:grayscale-0"
        />
        <div className="absolute inset-0 bg-black/30 group-hover:bg-transparent transition-colors duration-500" />
        
        <div className="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-all duration-500 scale-90 group-hover:scale-100">
          <div className="bg-white/10 backdrop-blur-md p-4 rounded-full border border-white/20 shadow-lg">
            {work.type === 'film' || work.type === 'dance' ? <Play size={24} fill="white" className="text-white" /> : <BookOpen size={24} className="text-white" />}
          </div>
        </div>
      </div>
      
      <div className="flex flex-col items-start px-1">
        <h3 className="text-lg font-serif text-white tracking-wider group-hover:text-neutral-200 transition-colors drop-shadow-md">
          {work.enTitle.toUpperCase()}
        </h3>
        <div className="flex items-center gap-2 mt-1 text-[10px] text-neutral-400 tracking-widest uppercase font-mono">
          <span>{work.title}</span>
          <span className="w-px h-3 bg-neutral-600"></span>
          <span>{work.year}</span>
        </div>
      </div>
    </div>
  );
};

const DetailModal = ({ work, onClose }) => {
  if (!work) return null;

  useEffect(() => {
    document.body.style.overflow = 'hidden';
    return () => { document.body.style.overflow = 'unset'; };
  }, []);

  const isVideo = work.type === 'film' || work.type === 'dance';

  return (
    <div className="fixed inset-0 z-[100] flex items-center justify-center px-0 md:px-8 py-0 md:py-8">
      <div 
        className="absolute inset-0 bg-black/90 backdrop-blur-xl animate-in fade-in duration-500"
        onClick={onClose}
      />

      <div className="relative w-full max-w-6xl h-full md:h-[85vh] bg-black/80 flex flex-col md:flex-row overflow-hidden shadow-2xl animate-in zoom-in-95 duration-500 border border-white/10">
        <button 
          onClick={onClose}
          className="absolute top-6 right-6 z-20 p-2 text-white/50 hover:text-white transition-colors bg-black/20 backdrop-blur rounded-full"
        >
          <X size={24} />
        </button>

        {/* Left: Visual/Media */}
        <div className="w-full md:w-[65%] bg-black/50 relative group flex items-center justify-center overflow-hidden">
          {isVideo ? (
            <>
              <img 
                src={work.image} 
                className="absolute inset-0 w-full h-full object-cover opacity-30 blur-xl scale-110"
                alt="background"
              />
              <div className="z-10 relative flex flex-col items-center justify-center p-12 w-full max-w-lg mx-auto">
                 <div className="aspect-video w-full bg-black shadow-2xl border border-white/10 relative overflow-hidden group-hover:border-white/30 transition-colors">
                    <img src={work.image} className="w-full h-full object-cover opacity-60" alt="poster" />
                    <div className="absolute inset-0 flex items-center justify-center">
                         {work.linkData?.url && work.linkData.url !== '#' ? (
                            <a 
                                href={work.linkData.url} 
                                target="_blank" 
                                rel="noopener noreferrer"
                                className="transform transition-transform duration-300 hover:scale-110"
                            >
                                <div className="w-20 h-20 bg-white/90 rounded-full flex items-center justify-center pl-1 cursor-pointer shadow-[0_0_30px_rgba(255,255,255,0.2)]">
                                    <Play size={32} fill="black" className="text-black" />
                                </div>
                            </a>
                         ) : (
                            <span className="text-white/50 font-mono text-xs border border-white/20 px-4 py-2">COMING SOON</span>
                         )}
                    </div>
                 </div>
                 
                 {work.linkData?.password && (
                    <div className="mt-8 font-mono text-xs tracking-widest text-neutral-400 flex items-center gap-3">
                        <span>PASSWORD:</span>
                        <span className="text-white bg-white/10 px-3 py-1 rounded select-all">{work.linkData.password}</span>
                    </div>
                 )}
              </div>
            </>
          ) : (
            <div className="w-full h-full p-8 md:p-16 flex items-center justify-center bg-black/50">
              <img src={work.image} alt={work.title} className="max-w-full max-h-full object-contain shadow-2xl" />
            </div>
          )}
        </div>

        {/* Right: Info */}
        <div className="w-full md:w-[35%] bg-black/90 p-8 md:p-12 overflow-y-auto border-l border-white/5 flex flex-col">
          <div className="mb-auto">
            <div className="flex items-baseline gap-3 mb-6">
                <span className="w-2 h-2 bg-white rounded-full"></span>
                <span className="text-[10px] font-mono text-neutral-500 uppercase tracking-[0.2em]">{work.format}</span>
            </div>
            
            <h2 className="text-3xl md:text-4xl font-serif text-white mb-2 leading-none tracking-wide">{work.enTitle}</h2>
            <h3 className="text-lg font-serif text-neutral-500 mb-8 tracking-widest">{work.title}</h3>

            <div className="space-y-6 text-sm leading-7 text-neutral-300 font-light tracking-wide text-justify">
                <p>{work.enDescription}</p>
                <div className="w-12 h-px bg-neutral-800"></div>
                <p className="text-neutral-400">{work.description}</p>
            </div>
          </div>

          {work.awards && (
            <div className="mt-12 pt-8 border-t border-white/5">
              <h4 className="text-[10px] font-bold text-neutral-600 uppercase tracking-[0.2em] mb-4">Selected / Awards</h4>
              <ul className="space-y-3">
                {work.awards.map((award, i) => (
                  <li key={i} className="text-xs text-neutral-400 font-light flex gap-3 leading-relaxed">
                    <span className="text-white">●</span> {award}
                  </li>
                ))}
              </ul>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

const About = () => (
  <div className="min-h-screen pt-32 pb-20 px-6 flex items-center justify-center animate-in fade-in slide-in-from-bottom-8 duration-1000">
    <div className="max-w-6xl w-full flex flex-col md:flex-row gap-16 md:gap-24 bg-black/60 backdrop-blur-sm p-8 md:p-12 border border-white/5 rounded-sm">
      {/* Left Column: Image & Contact */}
      <div className="w-full md:w-1/3 shrink-0 flex flex-col gap-8">
        <div className="relative aspect-[3/4] overflow-hidden bg-neutral-900 group shadow-2xl">
            {/* 建議使用 PDF 第 4 頁的人物照片 */}
            <img 
                src="https://placehold.co/600x800/111/FFF?text=Profile+Photo" 
                alt="TiaoTiao Yao" 
                className="w-full h-full object-cover grayscale contrast-110 opacity-80 group-hover:opacity-100 group-hover:grayscale-0 transition-all duration-1000 ease-out"
            />
            <div className="absolute inset-0 ring-1 ring-inset ring-white/10"></div>
        </div>
        
        <div className="space-y-6">
            <div>
                <h2 className="text-2xl font-serif text-white tracking-widest">姚易彤</h2>
                <p className="text-sm text-neutral-500 uppercase tracking-[0.3em] mt-2">TiaoTiao Yao</p>
            </div>
            
            <div className="space-y-2 font-mono text-xs text-neutral-400">
                <a href="mailto:tiaotiaofilm@qq.com" className="block hover:text-white transition-colors">tiaotiaofilm@qq.com</a>
                <p>Based in Xi'an / Beijing</p>
            </div>
        </div>
      </div>
      
      {/* Right Column: Text */}
      <div className="flex-1 space-y-12 text-neutral-300 font-light leading-loose text-justify pt-4">
        <div className="prose prose-invert prose-p:tracking-wide prose-p:text-neutral-300">
          <p className="first-letter:text-5xl first-letter:font-serif first-letter:text-white first-letter:mr-3 first-letter:float-left">
            曾用筆名條條、姚宇欣、張熱等。她是一位以電影創作為核心，融合多維藝術媒介的藝術創作者。
          </p>
          <p>
            姚易彤生於中國陝西西安，自幼習舞二十年，塑造了她對身體律動與能量流動的敏銳感知。同時，她對數學與邏輯同樣著迷，在理工與人文的雙重浸潤中，形成了獨特的觀察者視角，得以洞察世界之外的世界。
          </p>
          <p>
            2016年，姚易彤畢業於華中科技大學廣播電視學專業，在校期間還曾學習金融工程專業與建築設計專業。2017年，她任職於浙江萬達旅遊集團，負責多個國家的各類拍攝項目。
          </p>
          <p>
             2020年，她轉向自由創作，兼任導演、編劇、編舞等多重身份，深度參與電影、電視劇、廣告及紀錄片等項目，作品風格多元而富有探索精神。她的藝術實踐，綜合影像、詩歌、舞蹈、繪畫、音樂等多種媒介，共同構築出特有的<span className="text-white font-normal">「多維創作空間」(MULTIDIMENSIONAL CREATIVE SPACE)</span>。
          </p>
        </div>

        <div className="border-t border-white/10 pt-10">
          <h3 className="text-xs font-bold text-neutral-500 uppercase tracking-[0.2em] mb-6">Artist Statement</h3>
          <p className="italic text-neutral-400">
            "姚易彤對空間場域的能量變化，擁有極其細膩的感知與表達。她的靈感源自高維意識的覺知，以及對生命本源的體悟。通過不斷地自我覺察，她發展出了自己的創作母題——持續探索不同維度與時空的能量頻率，並用藝術實踐，將這股能量轉化為突破語言障礙的作品。以此，讓本源的頻率直抵人心，自然地構建人與人之間純粹而本真的連結。"
          </p>
        </div>
      </div>
    </div>
  </div>
);

// -----------------------------------------------------------------------------
// 主程序
// -----------------------------------------------------------------------------

export default function OfficialSite() {
  const [activeTab, setActiveTab] = useState('all');
  const [selectedWork, setSelectedWork] = useState(null);
  const [view, setView] = useState('home');

  const filteredWorks = activeTab === 'all' 
    ? WORKS 
    : WORKS.filter(w => w.type === activeTab);

  return (
    <div className="min-h-screen bg-black text-neutral-200 font-sans selection:bg-white selection:text-black overflow-x-hidden relative">
      {/* BACKGROUND LAYER */}
      <div className="fixed inset-0 z-0">
        <img 
            // 這裡使用了 Unsplash 的海洋/森林氛圍圖作為背景。
            // 您可以更換這個 URL 來改變網站的主背景。
            src="https://images.unsplash.com/photo-1500375592092-40eb2168fd21?q=80&w=2600&auto=format&fit=crop"
            alt="Sea and Forest Background" 
            className="w-full h-full object-cover opacity-40"
        />
        {/* 疊加深色漸變遮罩，確保文字清晰 */}
        <div className="absolute inset-0 bg-gradient-to-b from-black/80 via-black/60 to-black/90" />
        <div className="absolute inset-0 bg-black/20 backdrop-blur-[2px]" />
      </div>

      {/* CONTENT LAYER */}
      <div className="relative z-10">
        <Header activeTab={activeTab} setActiveTab={setActiveTab} setView={setView} />

        <main className="min-h-screen w-full">
            {view === 'home' ? (
            <>
                {/* Title / Intro Section */}
                <div className="pt-40 pb-16 px-6 text-center animate-in fade-in slide-in-from-top-8 duration-1000">
                    <p className="text-[10px] md:text-xs font-mono text-neutral-400 uppercase tracking-[0.4em] mb-4 drop-shadow-lg">
                        Multidimensional Creative Space
                    </p>
                </div>

                {/* Filter */}
                <div className="sticky top-20 z-30 bg-transparent backdrop-blur-sm border-b border-white/5 mb-16 transition-all">
                    <div className="flex justify-center overflow-x-auto scrollbar-hide py-6">
                        <div className="flex space-x-8 md:space-x-16 min-w-max px-6">
                            {CATEGORIES.map(cat => (
                            <button
                                key={cat.id}
                                onClick={() => setActiveTab(cat.id)}
                                className={`text-[10px] md:text-xs font-bold tracking-[0.2em] uppercase transition-all duration-500 relative group drop-shadow-md ${
                                activeTab === cat.id 
                                    ? 'text-white scale-105' 
                                    : 'text-neutral-400 hover:text-neutral-200'
                                }`}
                            >
                                {cat.label}
                                {activeTab === cat.id && (
                                    <span className="absolute -bottom-2 left-1/2 transform -translate-x-1/2 w-1 h-1 bg-white rounded-full shadow-[0_0_10px_white]"></span>
                                )}
                            </button>
                            ))}
                        </div>
                    </div>
                </div>

                {/* Grid */}
                <div className="max-w-[1600px] mx-auto px-4 md:px-12 pb-40">
                <div className="masonry-grid grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 md:gap-12">
                    {filteredWorks.map((work, index) => (
                    <div 
                        key={work.id} 
                        className="animate-in fade-in slide-in-from-bottom-12 duration-1000 fill-mode-backwards"
                        style={{ animationDelay: `${index * 100}ms` }}
                    >
                        <WorkCard work={work} onClick={setSelectedWork} />
                    </div>
                    ))}
                </div>
                
                {filteredWorks.length === 0 && (
                    <div className="h-64 flex items-center justify-center text-neutral-500 font-serif italic tracking-wider">
                    Content Loading...
                    </div>
                )}
                </div>
            </>
            ) : (
            <About />
            )}
        </main>

        {/* Footer */}
        <footer className="py-12 border-t border-white/5 bg-black/80 backdrop-blur-md">
            <div className="max-w-7xl mx-auto px-6 flex flex-col items-center gap-4">
                <div className="flex gap-6">
                    <a href="mailto:tiaotiaofilm@qq.com" className="w-8 h-8 rounded-full border border-white/10 flex items-center justify-center hover:bg-white hover:text-black transition-all cursor-pointer">
                        <Mail size={14} />
                    </a>
                </div>
                <p className="text-[10px] text-neutral-500 uppercase tracking-[0.2em]">
                © {new Date().getFullYear()} TiaoTiao Yao. All Rights Reserved.
                </p>
            </div>
        </footer>
      </div>

      {/* Modal */}
      {selectedWork && (
        <DetailModal work={selectedWork} onClose={() => setSelectedWork(null)} />
      )}
    </div>
  );
}
