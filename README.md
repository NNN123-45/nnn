# nnn
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Build Your Future 2026 - Personal Branding & OKRs</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-step { animation: fadeIn 0.4s ease-out forwards; }
    </style>
</head>
<body class="bg-slate-50">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;
        
        // Mocking Lucide icons for standalone HTML
        const Icon = ({ name, size = 20, className = "" }) => {
            return <i data-lucide={name} style={{ width: size, height: size }} className={className}></i>;
        };

        const App = () => {
            const [activeStep, setActiveStep] = useState(1);
            const [isSubmitted, setIsSubmitted] = useState(false);
            const [showGuide, setShowGuide] = useState(false);
            
            const [formData, setFormData] = useState({
                personalInfo: { fullName: '', department: '', position: '', period: 'Quý 1 - 2026' },
                branding: { coreValues: ['', '', ''], usp: '', targetImage: '' },
                okrs: [{ id: 1, objective: '', keyResult: '', deadline: '2026-03-31', progress: 0 }],
                actionPlan: [{ id: 1, action: '', resource: '', status: 'Pending' }],
                skills: { technical: '', soft: '', learningResources: '' }
            });

            useEffect(() => {
                if (window.lucide) {
                    window.lucide.createIcons();
                }
            }, [activeStep, isSubmitted, showGuide]);

            const valueSuggestions = [
                { category: 'Thái độ', items: ['Trách nhiệm', 'Chủ động', 'Chân thành', 'Kiên trì', 'Lạc quan'] },
                { category: 'Hiệu suất', items: ['Tốc độ', 'Chính xác', 'Sáng tạo', 'Kỷ luật', 'Thực thi'] },
                { category: 'Hợp tác', items: ['Thấu hiểu', 'Chia sẻ', 'Tận tâm', 'Lắng nghe', 'Truyền cảm hứng'] }
            ];

            const steps = [
                { id: 1, title: 'Thông tin chung', icon: 'user' },
                { id: 2, title: 'Branding', icon: 'award' },
                { id: 3, title: 'OKRs', icon: 'target' },
                { id: 4, title: 'Hành động', icon: 'rocket' },
                { id: 5, title: 'Năng lực', icon: 'book-open' }
            ];

            const handleNext = () => setActiveStep(prev => Math.min(prev + 1, 5));
            const handlePrev = () => setActiveStep(prev => Math.max(prev - 1, 1));

            const updateBranding = (field, value, index = null) => {
                if (index !== null) {
                    const newValues = [...formData.branding.coreValues];
                    newValues[index] = value;
                    setFormData({ ...formData, branding: { ...formData.branding, coreValues: newValues } });
                } else {
                    setFormData({ ...formData, branding: { ...formData.branding, [field]: value } });
                }
            };

            const calculateCompletion = () => {
                let score = 0;
                if (formData.personalInfo.fullName) score += 20;
                if (formData.branding.coreValues.every(v => v !== '')) score += 20;
                if (formData.okrs[0].objective) score += 20;
                if (formData.actionPlan[0].action) score += 20;
                if (formData.skills.technical) score += 20;
                return score;
            };

            if (isSubmitted) {
                return (
                    <div className="min-h-screen flex items-center justify-center p-6 bg-slate-100">
                        <div className="max-w-2xl w-full bg-white rounded-[2.5rem] shadow-2xl p-10 text-center animate-step">
                            <div className="w-20 h-20 bg-green-500 text-white rounded-full flex items-center justify-center mx-auto mb-6">
                                <Icon name="check-circle" size={40} />
                            </div>
                            <h1 className="text-3xl font-black mb-4 uppercase">Kế hoạch đã sẵn sàng!</h1>
                            <p className="text-slate-500 mb-8">Hệ thống đã ghi nhận lộ trình phát triển năm 2026 của {formData.personalInfo.fullName || 'bạn'}.</p>
                            <button onClick={() => window.print()} className="px-8 py-4 bg-blue-600 text-white rounded-2xl font-bold hover:bg-blue-700 transition w-full mb-4 flex items-center justify-center gap-2">
                                <Icon name="download" /> TẢI BẢN KẾ HOẠCH (PDF)
                            </button>
                            <button onClick={() => setIsSubmitted(false)} className="text-slate-400 font-bold hover:text-slate-600">Quay lại chỉnh sửa</button>
                        </div>
                    </div>
                );
            }

            return (
                <div className="max-w-5xl mx-auto py-10 px-4">
                    <header className="mb-10 flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                        <div>
                            <h1 className="text-4xl font-black text-slate-900 tracking-tight">BUILD YOUR <span className="text-blue-600">FUTURE</span></h1>
                            <p className="text-slate-500 font-medium">Lộ trình cá nhân hóa năm 2026</p>
                        </div>
                        <div className="bg-white p-4 rounded-2xl shadow-sm border border-slate-100 flex items-center gap-4">
                             <div className="text-right">
                                <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Độ hoàn thiện</p>
                                <p className="text-2xl font-black text-blue-600">{calculateCompletion()}%</p>
                             </div>
                        </div>
                    </header>

                    <nav className="flex gap-2 mb-8 overflow-x-auto no-scrollbar pb-2">
                        {steps.map(s => (
                            <button key={s.id} onClick={() => setActiveStep(s.id)} className={`flex items-center gap-2 px-5 py-3 rounded-xl transition-all min-w-max font-bold ${activeStep === s.id ? 'bg-slate-900 text-white shadow-lg' : 'bg-white text-slate-400 border border-slate-100'}`}>
                                <Icon name={s.icon} size={18} />
                                <span className="text-sm">{s.title}</span>
                            </button>
                        ))}
                    </nav>

                    <main className="bg-white rounded-[2rem] shadow-xl p-8 md:p-10 border border-slate-100 min-h-[500px] animate-step">
                        {activeStep === 1 && (
                            <div className="space-y-6">
                                <h2 className="text-xl font-bold flex items-center gap-2"><Icon name="user" className="text-blue-600"/> Thông tin cá nhân</h2>
                                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                                    <input type="text" placeholder="Họ và tên" className="p-4 border rounded-xl outline-none focus:ring-2 focus:ring-blue-500 transition" value={formData.personalInfo.fullName} onChange={e => setFormData({...formData, personalInfo: {...formData.personalInfo, fullName: e.target.value}})} />
                                    <input type="text" placeholder="Phòng ban" className="p-4 border rounded-xl outline-none focus:ring-2 focus:ring-blue-500 transition" value={formData.personalInfo.department} onChange={e => setFormData({...formData, personalInfo: {...formData.personalInfo, department: e.target.value}})} />
                                </div>
                            </div>
                        )}

                        {activeStep === 2 && (
                            <div className="space-y-6">
                                <div className="flex justify-between items-center">
                                    <h2 className="text-xl font-bold flex items-center gap-2 text-amber-600"><Icon name="award"/> Thương hiệu cá nhân 2026</h2>
                                    <button onClick={() => setShowGuide(!showGuide)} className="text-xs font-bold text-amber-600 flex items-center gap-1"><Icon name="lightbulb" size={14}/> Hướng dẫn chọn?</button>
                                </div>
                                {showGuide && (
                                    <div className="p-4 bg-amber-50 border border-amber-100 rounded-xl text-xs text-amber-800 space-y-1">
                                        <p><strong>1. Chân thực:</strong> Chọn điều bạn thực sự tin tưởng.</p>
                                        <p><strong>2. Định hướng:</strong> Giúp bạn ra quyết định khi gặp khó khăn.</p>
                                        <p><strong>3. Khác biệt:</strong> Tạo nên màu sắc riêng cho bạn.</p>
                                    </div>
                                )}
                                <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                                    {formData.branding.coreValues.map((v, i) => (
                                        <input key={i} type="text" placeholder={`Giá trị ${i+1}`} className="p-4 border bg-amber-50/30 rounded-xl outline-none focus:ring-2 focus:ring-amber-500 transition" value={v} onChange={e => updateBranding('coreValues', e.target.value, i)} />
                                    ))}
                                </div>
                                <div className="flex flex-wrap gap-2 items-center">
                                    <span className="text-[10px] font-black text-slate-400 uppercase">Gợi ý nhanh:</span>
                                    {valueSuggestions[0].items.map(item => (
                                        <button key={item} onClick={() => {
                                            const idx = formData.branding.coreValues.findIndex(v => v === '');
                                            if (idx !== -1) updateBranding('coreValues', item, idx);
                                        }} className="text-[10px] px-2 py-1 bg-white border rounded hover:border-amber-500 transition">+ {item}</button>
                                    ))}
                                </div>
                                <textarea placeholder="Thế mạnh vượt trội (USP)..." className="w-full p-4 border rounded-xl h-32 focus:ring-2 focus:ring-amber-500 outline-none transition" value={formData.branding.usp} onChange={e => updateBranding('usp', e.target.value)}></textarea>
                            </div>
                        )}

                        {activeStep > 2 && <div className="flex flex-col items-center justify-center py-20 text-slate-400 italic">Tính năng {steps[activeStep-1].title} hiện đang được cập nhật trong bản offline...</div>}
                    </main>

                    <footer className="mt-8 flex justify-between items-center bg-white/80 p-4 rounded-2xl shadow-lg border border-white">
                        <button onClick={handlePrev} className={`px-6 py-3 font-bold ${activeStep === 1 ? 'text-slate-200' : 'text-slate-600'}`}>QUAY LẠI</button>
                        {activeStep === 5 ? (
                            <button onClick={() => setIsSubmitted(true)} className="px-8 py-3 bg-blue-600 text-white rounded-xl font-bold shadow-lg shadow-blue-200">XUẤT BẢN</button>
                        ) : (
                            <button onClick={handleNext} className="px-8 py-3 bg-slate-900 text-white rounded-xl font-bold shadow-lg shadow-slate-200">TIẾP THEO</button>
                        )}
                    </footer>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
