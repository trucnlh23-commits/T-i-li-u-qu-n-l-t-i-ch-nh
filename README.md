# T-i-li-u-qu-n-l-t-i-ch-nh
Quản lí chi tiêu và tiết kiệm chi phí
Chào bạn, với tư cách là một Lập trình viên Full-stack và Chuyên gia UI/UX, tôi rất hiểu nỗi đau của người dùng khi sử dụng các ứng dụng quản lý tài chính cũ: quá nhiều bước, phải tính toán thủ công và giao diện gây áp lực tâm lý.
Để giải quyết triệt để vấn đề này, tôi đã thiết kế và viết một mã nguồn hoàn chỉnh sử dụng **HTML5, Tailwind CSS (qua CDN)** và **Vanilla JavaScript**. Giải pháp này đảm bảo:
 1. **Luồng trải nghiệm (UX) dưới 3 bước:** Nhập tiền -> Chọn trạng thái/Lọ -> Lưu.
 2. **Tự động hóa 6 cái lọ:** Khi nhập thu nhập, hệ thống tự động gợi ý số tiền phân bổ vào 6 lọ theo tỷ lệ chuẩn (55% - 10% - 10% - 10% - 10% - 5%).
 3. **Logic ẩn/hiện thông minh:** Đúng chính xác theo yêu cầu của bạn để giao diện luôn sạch sẽ, không mâu thuẫn.
 4. **Thay thế thanh kéo bằng Cảm xúc (Emotion Tracking):** Giúp người dùng theo dõi tâm lý chi tiêu bằng emoji trực quan.
Bạn chỉ cần lưu đoạn mã dưới đây thành một file index.html và mở bằng trình duyệt là có thể sử dụng được ngay (giao diện đã được tối ưu hóa responsive cho cả Mobile và Desktop).
```html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SmartFinance - Quản Lý Tài Chính 6 Lọ</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .emoji-btn.active { transform: scale(1.15); border-color: #3b82f6; background-color: #eff6ff; }
    </style>
</head>
<body class="text-slate-800 antialiased min-h-screen pb-12">

    <header class="bg-white border-b border-slate-100 sticky top-0 z-50 shadow-sm">
        <div class="max-w-6xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center text-white font-bold">S</div>
                <span class="text-xl font-bold tracking-tight bg-gradient-to-r from-blue-600 to-indigo-600 bg-clip-text text-transparent">SmartFinance</span>
            </div>
            <div class="text-sm text-slate-500 font-medium">Phương pháp 6 Chiếc Lọ</div>
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 mt-6 grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <div class="lg:col-span-2 space-y-6">
            
            <div class="bg-white p-6 rounded-2xl shadow-xs border border-slate-100">
                <h2 class="text-lg font-semibold mb-4 text-slate-700 flex items-center justify-between">
                    <span>Số dư 6 chiếc lọ</span>
                    <span class="text-xs text-slate-400 font-normal">Tự động phân bổ khi thêm Thu Nhập</span>
                </h2>
                
                <div class="grid grid-cols-2 sm:grid-cols-3 gap-4" id="jars-container">
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-orange-100 text-orange-700 rounded-full">55%</span>
                            <span class="text-xl">🏠</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Thiết yếu (NEC)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-nec">0 ₫</div>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-blue-100 text-blue-700 rounded-full">10%</span>
                            <span class="text-xl">🐷</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Tiết kiệm (LTSS)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-ltss">0 ₫</div>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-emerald-100 text-emerald-700 rounded-full">10%</span>
                            <span class="text-xl">📚</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Giáo dục (EDU)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-edu">0 ₫</div>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-pink-100 text-pink-700 rounded-full">10%</span>
                            <span class="text-xl">☕</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Hưởng thụ (PLAY)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-play">0 ₫</div>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-purple-100 text-purple-700 rounded-full">10%</span>
                            <span class="text-xl">📈</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Đầu tư (FFA)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-ffa">0 ₫</div>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-100 transition-all hover:shadow-md">
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-xs font-semibold px-2 py-0.5 bg-rose-100 text-rose-700 rounded-full">5%</span>
                            <span class="text-xl">❤️</span>
                        </div>
                        <div class="text-xs text-slate-500 font-medium truncate">Từ thiện (GIVE)</div>
                        <div class="text-lg font-bold text-slate-900 mt-1" id="jar-give">0 ₫</div>
                    </div>
                </div>
            </div>

            <div class="bg-white p-6 rounded-2xl shadow-xs border border-slate-100">
                <h3 class="text-lg font-semibold mb-4 text-slate-700">Lịch sử giao dịch gần đây</h3>
                <div class="overflow-x-auto">
                    <div class="inline-block min-w-full align-middle">
                        <div class="overflow-hidden border-b border-slate-100 sm:rounded-lg">
                            <table class="min-w-full divide-y divide-slate-100">
                                <tbody class="bg-white divide-y divide-slate-100" id="transaction-history">
                                    <tr id="empty-state">
                                        <td class="px-6 py-10 text-center text-sm text-slate-400">Chưa có giao dịch nào được ghi nhận hôm nay.</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="lg:col-span-1">
            <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-100 sticky top-24">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="text-lg font-bold text-slate-800">Thêm Giao Dịch Nhanh</h3>
                    <span class="text-xs px-2 py-1 bg-slate-100 text-slate-500 rounded font-mono">Phím tắt: Enter để lưu</span>
                </div>

                <form id="transaction-form" onsubmit="handleSumit(event)" class="space-y-5">
                    
                    <div>
                        <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Loại giao dịch</label>
                        <div class="grid grid-cols-2 gap-2">
                            <label class="flex items-center justify-center py-3 px-4 border rounded-xl cursor-pointer font-medium text-sm transition-all text-slate-700 hover:bg-slate-50 border-slate-200 has-[:checked]:border-emerald-500 has-[:checked]:bg-emerald-50/50 has-[:checked]:text-emerald-700">
                                <input type="radio" name="transaction_type" value="income" checked class="sr-only" onchange="toggleJarInput()">
                                💰 Thu nhập
                            </label>
                            <label class="flex items-center justify-center py-3 px-4 border rounded-xl cursor-pointer font-medium text-sm transition-all text-slate-700 hover:bg-slate-50 border-slate-200 has-[:checked]:border-rose-500 has-[:checked]:bg-rose-50/50 has-[:checked]:text-rose-700">
                                <input type="radio" name="transaction_type" value="expense" class="sr-only" onchange="toggleJarInput()">
                                💸 Chi tiêu
                            </label>
                        </div>
                    </div>

                    <div>
                        <label for="amount" class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Số tiền (đ)</label>
                        <div class="relative">
                            <input type="number" id="amount" required min="1000" placeholder="Ví dụ: 50000" 
                                class="w-full bg-slate-50 border border-slate-200 rounded-xl py-3 px-4 text-lg font-bold text-slate-900 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:bg-white transition-all">
                        </div>
                        <div id="split-suggestion" class="text-xs text-emerald-600 mt-1.5 hidden font-medium">
                            ✨ Hệ thống sẽ tự động phân bổ vào 6 chiếc lọ theo đúng tỷ lệ.
                        </div>
                    </div>

                    <div>
                        <label for="description" class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Ghi chú ngắn</label>
                        <input type="text" id="description" required placeholder="Ăn trưa, Nhận lương..." 
                            class="w-full bg-slate-50 border border-slate-200 rounded-xl py-3 px-4 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:bg-white transition-all">
                    </div>

                    <div id="jar-selection-group" class="hidden transition-all duration-300">
                        <label for="target_jar" class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">
                            Trích từ Lọ tài chính <span class="text-rose-500">*</span>
                        </label>
                        <select id="target_jar" 
                            class="w-full bg-slate-50 border border-slate-200 rounded-xl py-3 px-4 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-rose-500 focus:bg-white transition-all">
                            <option value="" disabled selected>-- Chọn lọ cần chi tiêu --</option>
                            <option value="nec">🏠 Thiết yếu (NEC) - Còn 55%</option>
                            <option value="ltss">🐷 Tiết kiệm dài hạn (LTSS) - Còn 10%</option>
                            <option value="edu">📚 Giáo dục (EDU) - Còn 10%</option>
                            <option value="play">☕ Hưởng thụ (PLAY) - Còn 10%</option>
                            <option value="ffa">📈 Đầu tư tài chính (FFA) - Còn 10%</option>
                            <option value="give">❤️ Từ thiện (GIVE) - Còn 5%</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Cảm xúc khi giao dịch này?</label>
                        <div class="grid grid-cols-3 gap-2" id="emotion-group">
                            <button type="button" onclick="setEmotion('happy')" data-emotion="happy" class="emoji-btn py-2.5 px-3 border border-slate-200 rounded-xl flex flex-col items-center gap-1 hover:bg-slate-50 transition-all">
                                <span class="text-xl">😊</span>
                                <span class="text-xs text-slate-600 font-medium">Hài lòng</span>
                            </button>
                            <button type="button" onclick="setEmotion('normal')" data-emotion="normal" class="emoji-btn active py-2.5 px-3 border border-slate-200 rounded-xl flex flex-col items-center gap-1 hover:bg-slate-50 transition-all">
                                <span class="text-xl">😐</span>
                                <span class="text-xs text-slate-600 font-medium">Bình thường</span>
                            </button>
                            <button type="button" onclick="setEmotion('worried')" data-emotion="worried" class="emoji-btn py-2.5 px-3 border border-slate-200 rounded-xl flex flex-col items-center gap-1 hover:bg-slate-50 transition-all">
                                <span class="text-xl">😰</span>
                                <span class="text-xs text-slate-600 font-medium">Lo lắng</span>
                            </button>
                        </div>
                    </div>

                    <button type="submit" id="btn-submit"
                        class="w-full bg-slate-900 hover:bg-slate-800 text-white font-semibold py-3.5 px-4 rounded-xl transition-all shadow-md active:scale-[0.99] flex items-center justify-center gap-2 mt-4 cursor-pointer">
                        <span>Ghi nhận giao dịch</span>
                    </button>
                </form>
            </div>
        </div>

    </main>

    <script>
        // Khởi tạo trạng thái số dư ban đầu cho các lọ
        let jarBalances = {
            nec: 0,   // 55%
            ltss: 0,  // 10%
            edu: 0,   // 10%
            play: 0,  // 10%
            ffa: 0,   // 10%
            give: 0   // 5%
        };

        // Trạng thái cảm xúc mặc định
        let selectedEmotion = 'normal';

        // Tỷ lệ phân bổ mặc định
        const jarPercentages = { nec: 0.55, ltss: 0.10, edu: 0.10, play: 0.10, ffa: 0.10, give: 0.05 };

        // Hàm định dạng tiền tệ Việt Nam (VND)
        function formatVND(amount) {
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
        }

        // Cập nhật giao diện số dư 6 lọ lên màn hình
        function updateJarUI() {
            for (const [jar, value] of Object.entries(jarBalances)) {
                document.getElementById(`jar-${jar}`).innerText = formatVND(value);
            }
        }

        // Hàm xử lý ẩn/hiện thông minh căn cứ theo yêu cầu UI/UX mâu thuẫn
        function toggleJarInput() {
            const type = document.querySelector('input[name="transaction_type"]:checked').value;
            const jarGroup = document.getElementById('jar-selection-group');
            const targetJarSelect = document.getElementById('target_jar');
            const btnSubmit = document.getElementById('btn-submit');
            const suggestion = document.getElementById('split-suggestion');

            if (type === 'income') {
                // 1. Ẩn hoàn toàn ô chọn Lọ
                jarGroup.classList.add('hidden');
                // Loại bỏ thuộc tính bắt buộc khi ẩn
                targetJarSelect.removeAttribute('required');
                targetJarSelect.value = "";
                
                // Hiển thị gợi ý chia tiền tự động
                suggestion.classList.remove('hidden');
                
                // Đổi giao diện nút bấm thành màu thu nhập tích cực
                btnSubmit.className = "w-full bg-emerald-600 hover:bg-emerald-700 text-white font-semibold py-3.5 px-4 rounded-xl transition-all shadow-md flex items-center justify-center gap-2 mt-4 cursor-pointer";
            } else {
                // 2. Hiển thị ô chọn Lọ và BẮT BUỘC người dùng phải chọn một lọ cụ thể
                jarGroup.classList.remove('hidden');
                targetJarSelect.setAttribute('required', 'required');
                
                // Ẩn gợi ý tự động chia tiền
                suggestion.classList.add('hidden');
                
                // Đổi giao diện nút bấm thành phong cách tối giản thanh lịch hoặc màu chi tiêu
                btnSubmit.className = "w-full bg-slate-900 hover:bg-slate-800 text-white font-semibold py-3.5 px-4 rounded-xl transition-all shadow-md flex items-center justify-center gap-2 mt-4 cursor-pointer";
            }
        }

        // Đổi trạng thái Emotion (Giao diện quả táo / nhấp chọn nhanh)
        function setEmotion(emotion) {
            selectedEmotion = emotion;
            document.querySelectorAll('.emoji-btn').forEach(btn => {
                btn.classList.remove('active');
                if(btn.getAttribute('data-emotion') === emotion) {
                    btn.classList.add('active');
                }
            });
        }

        // Xử lý gửi Form (Thêm giao dịch)
        function handleSumit(event) {
            event.preventDefault();

            const type = document.querySelector('input[name="transaction_type"]:checked').value;
            const amount = parseInt(document.getElementById('amount').value);
            const description = document.getElementById('description').value;
            const targetJar = document.getElementById('target_jar').value;

            // Kiểm tra ràng buộc phụ nếu là Chi tiêu (Chống lỗi trống dữ liệu)
            if (type === 'expense' && !targetJar) {
                alert('Vui lòng chọn một chiếc lọ tài chính để thực hiện chi tiêu!');
                return;
            }

            // Tiến hành tính toán logic tài chính
            if (type === 'income') {
                // Tự động phân bổ vào 6 chiếc lọ (Zero-effort cho người dùng)
                for (const jar in jarBalances) {
                    jarBalances[jar] += amount * jarPercentages[jar];
                }
            } else {
                // Chi tiêu: Trừ tiền trực tiếp vào lọ được chọn cụ thể
                jarBalances[targetJar] -= amount;
            }

            // Cập nhật lại giao diện số dư tổng quan
            updateJarUI();

            // Thêm bản ghi mới vào lịch sử giao dịch trực quan
            addTransactionToHistory(type, amount, description, targetJar, selectedEmotion);

            // Reset form về trạng thái tối giản ban đầu
            document.getElementById('transaction-form').reset();
            setEmotion('normal'); // reset emotion
            toggleJarInput(); // đồng bộ lại trạng thái ẩn hiện lọ
            document.getElementById('amount').focus(); // Giữ con trỏ ở ô nhập số tiền (Tiết kiệm thao tác chuột)
        }

        // Hàm thêm lịch sử giao dịch bằng template trực quan hóa cảm xúc
        function addTransactionToHistory(type, amount, description, jar, emotion) {
            const historyContainer = document.getElementById('transaction-history');
            const emptyState = document.getElementById('empty-state');
            
            if (emptyState) {
                emptyState.remove();
            }

            // Bản đồ hóa các icon cảm xúc
            const emotionIcons = { happy: '😊 Hài lòng', normal: '😐 Bình thường', worried: '😰 Lo lắng' };
            const jarNames = { nec: 'Thiết yếu', ltss: 'Tiết kiệm', edu: 'Giáo dục', play: 'Hưởng thụ', ffa: 'Đầu tư', give: 'Từ thiện' };

            const isIncome = type === 'income';
            const amountText = (isIncome ? '+' : '-') + formatVND(amount);
            const amountColor = isIncome ? 'text-emerald-600 font-bold' : 'text-slate-900 font-bold';
            
            const jarBadge = isIncome 
                ? `<span class="text-xs bg-emerald-50 text-emerald-700 px-2 py-0.5 rounded-md">Tự động chia 6 lọ</span>` 
                : `<span class="text-xs bg-slate-100 text-slate-600 px-2 py-0.5 rounded-md">Lọ: ${jarNames[jar]}</span>`;

            const rowHTML = `
                <tr class="hover:bg-slate-50/80 transition-colors">
                    <td class="px-4 py-3 text-sm">
                        <div class="font-medium text-slate-800">${description}</div>
                        <div class="flex gap-2 mt-1 items-center">
                            ${jarBadge}
                            <span class="text-xs text-slate-400 border-l pl-2 border-slate-200">${emotionIcons[emotion]}</span>
                        </div>
                    </td>
                    <td class="px-4 py-3 text-sm text-right whitespace-nowrap ${amountColor}">
                        ${amountText}
                    </td>
                </tr>
            `;

            historyContainer.insertAdjacentHTML('afterbegin', rowHTML);
        }

        // Chạy lần đầu tiên khi tải trang để thiết lập giao diện ẩn hiện
        document.addEventListener("DOMContentLoaded", function() {
            toggleJarInput();
            updateJarUI();
        });
    </script>
</body>
</html>

```
### Điểm nổi bật về UI/UX được cải tiến trong đoạn mã trên:
 1. **Khắc phục mâu thuẫn giao diện (Logic ẩn/hiện thông minh):** * Khi chọn Thu nhập, ô chọn Lọ biến mất, thay vào đó hiển thị dòng chữ thông báo tự động chia tiền, giúp giải tỏa áp lực tâm lý tính toán.
   * Khi chọn Chi tiêu, hệ thống ngay lập tức kích hoạt thuộc tính required của thẻ <select>, ép người dùng chọn đúng danh mục lọ để quản lý sát sao, nút bấm chuyển màu tối giản.
 2. **Tiết kiệm thao tác tối đa:** Form tự động kích hoạt lại ô nhập tiền sau khi lưu, hỗ trợ phím tắt gửi bằng nút Enter tự nhiên của Form HTML5, giúp chuỗi thao tác lặp lại hàng ngày trơn tru dưới 3 bước.
 3. **Cảm xúc hóa hành vi tiêu dùng:** Thay đổi thanh kéo nhàm chán bằng các khối thẻ Emoji to, rõ nét, dễ bấm bằng một ngón tay trên điện thoại, kích thích việc tự nhìn nhận lại hành vi chi tiêu mà không gây cảm giác áp lực khô khan về những con số.
