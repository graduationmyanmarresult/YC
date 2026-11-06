<html lang="my">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Yoon Copy Ebook Printing Service</title>
    <!-- Google Font (Pyidaungsu) -->
    <link href="https://fonts.googleapis.com/css2?family=Pyidaungsu&display=swap" rel="stylesheet">
    <!-- SheetJS (xlsx library for Excel Import/Export) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <!-- html2canvas library for saving receipt as JPEG -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <!-- Firebase App (Modular CDN with your config) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getDatabase, ref, set, onValue, push } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCun01cjEFWNHWe-iiZt0BukJffzhYA5uI",
            authDomain: "yoon-copy-2018.firebaseapp.com",
            databaseURL: "https://yoon-copy-2018-default-rtdb.firebaseio.com",
            projectId: "yoon-copy-2018",
            storageBucket: "yoon-copy-2018.appspot.com",
            messagingSenderId: "899376684115",
            appId: "1:899376684115:web:ba6f55b3b18564d5b50626"
        };

        const app = initializeApp(firebaseConfig);
        const db = getDatabase(app);

        window.fbDB = db;
        window.fbRef = ref;
        window.fbSet = set;
        window.fbOnValue = onValue;
        window.fbPush = push;
    </script>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: 'Pyidaungsu', sans-serif;
            background-color: #f8fafc;
            color: #1e293b;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        header {
            background-color: #4338ca;
            color: white;
            padding: 12px 24px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
        }
        .logo-container {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .logo-img {
            height: 48px;
            width: auto;
            background: white;
            padding: 4px 10px;
            border-radius: 10px;
            object-fit: contain;
            display: block;
        }
        .receipt-header-banner {
            width: 100% !important;
            max-width: 100% !important;
            height: auto !important;
            max-height: none !important;
            display: block;
            margin: 0 auto 6px auto;
            object-fit: cover !important;
        }
        header h1 { font-size: 20px; font-weight: bold; line-height: 1.2; }
        header p { font-size: 11px; color: #c7d2fe; }
        .nav-btns button {
            background-color: #3730a3;
            color: #e0e7ff;
            border: none;
            padding: 8px 16px;
            border-radius: 8px;
            font-family: 'Pyidaungsu', sans-serif;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
        }
        .nav-btns button.active {
            background-color: white;
            color: #4338ca;
        }
        main {
            flex: 1;
            max-width: 1280px;
            width: 100%;
            margin: 0 auto;
            padding: 20px;
        }
        .grid-container {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
        }
        @media (max-width: 1024px) {
            .grid-container { grid-template-columns: 1fr; }
        }
        .card {
            background: white;
            border-radius: 16px;
            padding: 20px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            border: 1px solid #e2e8f0;
            margin-bottom: 20px;
        }
        label { display: block; font-weight: bold; font-size: 14px; margin-bottom: 6px; color: #334155; }
        select, input {
            width: 100%;
            padding: 10px 12px;
            border: 1px solid #cbd5e1;
            border-radius: 10px;
            font-family: 'Pyidaungsu', sans-serif;
            font-size: 14px;
            outline: none;
            background-color: #f8fafc;
        }
        select:focus, input:focus { border-color: #4338ca; box-shadow: 0 0 0 3px rgba(67,56,202,0.1); }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th { background-color: #f1f5f9; color: #475569; font-size: 12px; text-transform: uppercase; padding: 10px; text-align: left; }
        td { padding: 10px; border-bottom: 1px solid #f1f5f9; font-size: 14px; vertical-align: middle; }
        .btn {
            background-color: #4338ca;
            color: white;
            border: none;
            padding: 8px 14px;
            border-radius: 8px;
            font-family: 'Pyidaungsu', sans-serif;
            font-weight: bold;
            cursor: pointer;
        }
        .btn-success { background-color: #059669; }
        .btn-success:hover { background-color: #047857; }
        .btn-danger { background-color: #e11d48; }
        .btn-warning { background-color: #d97706; }
        .btn-block { width: 100%; padding: 12px; font-size: 16px; }
        .hidden { display: none !important; }
        .modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(15, 23, 42, 0.6); backdrop-filter: blur(4px);
            display: flex; align-items: center; justify-content: center; z-index: 1000;
        }
        .modal-content {
            background: white; border-radius: 20px; width: 100%; max-width: 650px;
            overflow: hidden; box-shadow: 0 20px 25px -5px rgba(0,0,0,0.1);
        }
        .modal-header { background: #4338ca; color: white; padding: 16px 20px; display: flex; justify-content: space-between; align-items: center; }
        .modal-body { padding: 20px; max-height: 75vh; overflow-y: auto; }
        .modal-footer { padding: 16px 20px; background: #f8fafc; border-top: 1px solid #f1f5f9; display: flex; gap: 10px; flex-wrap: wrap; }

        .receipt-container-box {
            position: relative;
            background: #ffffff;
            border: 1px dashed #cbd5e1;
            padding: 15px;
            border-radius: 12px;
            font-size: 13px;
            overflow: hidden;
        }
        .receipt-container-box::before {
            content: "Yoon";
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(-30deg);
            font-size: 90px;
            font-weight: bold;
            color: rgba(217, 4, 41, 0.06);
            z-index: 0;
            pointer-events: none;
            white-space: nowrap;
            font-family: 'Pyidaungsu', sans-serif;
        }
        .receipt-inner-content {
            position: relative;
            z-index: 1;
        }

        .receipt-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 5px;
            margin-bottom: 5px;
        }
        .receipt-table th {
            border-bottom: 1px solid #000;
            padding: 4px;
            font-size: 12px;
            text-align: left;
            background: transparent;
            color: #000;
        }
        .receipt-table td {
            padding: 3px 4px;
            font-size: 12px;
            border-bottom: 1px dashed #e2e8f0;
            line-height: 1.2;
            color: #000;
        }

        @media print {
            body, html {
                background: white !important;
                margin: 0 !important;
                padding: 0 !important;
                height: auto !important;
                overflow: visible !important;
            }
            body * { visibility: hidden !important; }
            .modal-header, .modal-footer, select, label, input, .nav-btns, header, main, .header-change-btn, #edit-sale-items-list {
                display: none !important;
            }
            body.print-checkout #checkout-modal,
            body.print-checkout #printable-receipt,
            body.print-checkout #printable-receipt * {
                visibility: visible !important;
            }
            body.print-checkout #printable-receipt {
                position: absolute !important; left: 0 !important; top: 0 !important;
                width: 100% !important; margin: 0 !important; padding: 5px !important;
                border: none !important; background: white !important;
            }
            body.print-report #edit-sale-modal,
            body.print-report #printable-edit-receipt,
            body.print-report #printable-edit-receipt * {
                visibility: visible !important;
            }
            body.print-report #printable-edit-receipt {
                position: absolute !important; left: 0 !important; top: 0 !important;
                width: 100% !important; margin: 0 !important; padding: 5px !important;
                border: none !important; background: white !important;
            }
            .modal {
                position: absolute !important; left: 0 !important; top: 0 !important;
                width: 100% !important; height: auto !important; background: none !important;
                backdrop-filter: none !important; display: block !important;
            }
            .modal-content {
                box-shadow: none !important; border: none !important;
                max-width: 100% !important; width: 100% !important; margin: 0 !important; padding: 0 !important;
            }
            .receipt-table td { border-bottom: 1px solid #ddd !important; }
        }
    </style>
</head>
<body>

    <!-- Header -->
    <header>
        <div class="logo-container">
            <img id="header-logo" class="logo-img" alt="Yoon Logo">
            <div>
                <h1>Yoon Copy Ebook Printing Service</h1>
                <p>သင်တန်းကျောင်း စာအုပ်နှင့် ပစ္စည်းအရောင်းစနစ် (Real-time Cloud POS)</p>
            </div>
        </div>
        <div class="nav-btns">
            <button onclick="switchTab('pos')" id="tab-pos" class="active">အရောင်းပိုင်း (POS)</button>
            <button onclick="switchTab('reports')" id="tab-reports">အရောင်းစာရင်း (Reports)</button>
            <button onclick="switchTab('catalog')" id="tab-catalog">ကျောင်းနှင့် စာအုပ်မာစတာ</button>
        </div>
    </header>

    <!-- Main Container -->
    <main>

        <!-- ================= 1. POS SALES SECTION ================= -->
        <div id="section-pos" class="grid-container">
            
            <div>
                <div class="card">
                    <div style="margin-bottom: 12px;">
                        <label>၁။ သင်တန်းကျောင်း ရွေးချယ်ပါ</label>
                        <select id="pos-school-select" onchange="filterPOSBooks()">
                            <option value="">-- ကျောင်းရွေးချယ်ပါ --</option>
                        </select>
                    </div>
                    <div style="margin-bottom: 12px;">
                        <label>၂။ Customer Name (အမည် / ဖုန်းနံပါတ်)</label>
                        <input type="text" id="pos-customer-input" placeholder="ဥပမာ - မောင်မောင် (0912345678)...">
                    </div>
                    <div>
                        <label>၃။ စာအုပ်ရှာရန် (Book No. သို့မဟုတ် အမည်)</label>
                        <input type="text" id="pos-search-input" oninput="filterPOSBooks()" placeholder="Book No. (ဥပမာ - 1, 89) သို့မဟုတ် စာအုပ်အမည် ရိုက်ပါ...">
                    </div>
                </div>

                <!-- Custom / Free Sale Section -->
                <div class="card" style="background: #f8fafc; border: 1px dashed #cbd5e1;">
                    <h3 style="margin-bottom: 10px; font-size: 15px; color: #4338ca;">➕ စာအုပ်မာစတာတွင် မပါလျှင် အလွတ်ရိုက်ထည့်၍ ရောင်းမည်</h3>
                    <div style="display: grid; grid-template-columns: 2fr 1fr 1fr; gap: 10px; margin-bottom: 10px;">
                        <input type="text" id="free-book-title" placeholder="ပစ္စည်း/စာအုပ် အမည် ရိုက်ရန်...">
                        <input type="number" id="free-book-price" placeholder="ဈေးနှုန်း (ကျပ်)">
                        <input type="number" id="free-book-qty" value="1" min="1" placeholder="ခုရေ">
                    </div>
                    <button onclick="addFreeItemToCart()" class="btn btn-success" style="font-size: 13px; padding: 6px 12px;">အလွတ်စာရင်း ထည့်မည် (+)</button>
                </div>

                <div class="card">
                    <h3 style="margin-bottom: 12px; font-size: 16px; color: #334155;">ရရှိနိုင်သော စာအုပ်များစာရင်း (အုပ်ရေ တိုက်ရိုက်ရိုက်ထည့်နိုင်သည်)</h3>
                    <div style="overflow-x-auto;">
                        <table>
                            <thead>
                                <tr>
                                    <th>No.</th>
                                    <th>စာအုပ်/ပစ္စည်း အမည်</th>
                                    <th style="text-align: right;">၁ အုပ်/ခု ဈေးနှုန်း</th>
                                    <th style="text-align: center;">အုပ်ရေ</th>
                                    <th style="text-align: center;">လုပ်ဆောင်ချက်</th>
                                </tr>
                            </thead>
                            <tbody id="pos-book-list">
                                <tr><td colspan="5" style="text-align: center; color: #94a3b8; padding: 20px;">ကျေးဇူးပြု၍ ကျောင်းတစ်ကျောင်း ရွေးချယ်ပါ။</td></tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <div class="card" style="display: flex; flex-direction: column; justify-content: space-between;">
                <div>
                    <h3 style="margin-bottom: 12px; font-size: 16px; color: #334155; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px;">၄။ ဝယ်ယူမည့် စာရင်း (Cart)</h3>
                    <div id="cart-items" style="max-height: 350px; overflow-y: auto; margin-bottom: 15px;">
                        <p style="color: #94a3b8; font-size: 13px; text-align: center; padding: 30px 0;">စာအုပ် ရွေးချယ်ထားခြင်း မရှိသေးပါ။</p>
                    </div>
                </div>

                <div style="border-top: 1px solid #f1f5f9; padding-top: 15px;">
                    <div style="display: flex; justify-content: space-between; font-size: 18px; font-weight: bold; margin-bottom: 15px;">
                        <span>စုစုပေါင်း ကျသင့်ငွေ:</span>
                        <span id="cart-total" style="color: #4338ca;">0 ကျပ်</span>
                    </div>
                    <button onclick="openCheckoutModal()" class="btn btn-success btn-block">ငွေရှင်းမည် / ဘောင်ချာထုတ်မည်</button>
                </div>
            </div>

        </div>

        <!-- ================= 2. SALES REPORTS SECTION ================= -->
        <div id="section-reports" class="card hidden">
            <h2 style="font-size: 18px; font-weight: bold; color: #4338ca; margin-bottom: 15px;">📊 အရောင်းစာရင်းများနှင့် ဘောင်ချာမှတ်တမ်းများ ရှာဖွေရန်</h2>
            
            <div style="display: flex; flex-wrap: wrap; gap: 15px; margin-bottom: 20px; align-items: flex-end;">
                <div>
                    <label>ကျောင်းအလိုက် စစ်ဆေးရန်</label>
                    <select id="report-school-filter" onchange="generateReport()">
                        <option value="all">-- ကျောင်းအားလုံး --</option>
                    </select>
                </div>
                <div style="flex: 1; min-width: 250px;">
                    <label>Customer Name သို့မဟုတ် Invoice ဖြင့် ရှာရန်</label>
                    <input type="text" id="filter-invoice-input" oninput="generateReport()" placeholder="Customer Name သို့မဟုတ် Invoice No... ">
                </div>
                <button onclick="generateReport()" class="btn">စာရင်းရှာမည်</button>
            </div>

            <!-- SCHOOL-WISE SALES SUMMARY TABLE -->
            <div style="margin-bottom: 25px; background: #f8fafc; padding: 15px; border-radius: 12px; border: 1px solid #e2e8f0;">
                <h3 style="font-size: 15px; font-weight: bold; color: #4338ca; margin-bottom: 10px;">🏫 ကျောင်းအလိုက် ရောင်းချရမှု စာရင်း (School Summary)</h3>
                <div style="overflow-x-auto;">
                    <table>
                        <thead>
                            <tr>
                                <th>ကျောင်းအမည်</th>
                                <th style="text-align: center;">ရောင်းချရသည့် စာအုပ်/ပစ္စည်း အရေအတွက်</th>
                                <th style="text-align: right;">စုစုပေါင်း ရောင်းရငွေ</th>
                            </tr>
                        </thead>
                        <tbody id="school-sales-summary-body">
                            <tr><td colspan="3" style="text-align: center; color: #94a3b8;">စာရင်း မရှိသေးပါ။</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- CUSTOMER-WISE SALES SUMMARY TABLE -->
            <div style="margin-bottom: 25px; background: #f8fafc; padding: 15px; border-radius: 12px; border: 1px solid #e2e8f0;">
                <h3 style="font-size: 15px; font-weight: bold; color: #059669; margin-bottom: 10px;">👤 Customer တစ်ဦးချင်းအလိုက် ဝယ်ယူမှု စာရင်း (Customer Summary)</h3>
                <div style="overflow-x-auto;">
                    <table>
                        <thead>
                            <tr>
                                <th>Customer Name</th>
                                <th style="text-align: center;">ဝယ်ယူသည့် ဘောင်ချာ အရေအတွက်</th>
                                <th style="text-align: right;">စုစုပေါင်း ဝယ်ယူမှု ပမာဏ</th>
                            </tr>
                        </thead>
                        <tbody id="customer-sales-summary-body">
                            <tr><td colspan="3" style="text-align: center; color: #94a3b8;">စာရင်း မရှိသေးပါ။</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <h3 style="font-size: 16px; font-weight: bold; margin-bottom: 10px; color: #334155;">🧾 ဘောင်ချာအသေးစိတ် မှတ်တမ်းများ</h3>
            <div style="overflow-x-auto;">
                <table>
                    <thead>
                        <tr>
                            <th>ရက်စွဲနှင့် အချိန်</th>
                            <th>Invoice နံပါတ်</th>
                            <th>Customer Name</th>
                            <th style="text-align: right;">ကျသင့်ငွေ</th>
                            <th style="text-align: center;">အခြေအနေ</th>
                            <th style="text-align: center;">လုပ်ဆောင်ချက်</th>
                        </tr>
                    </thead>
                    <tbody id="report-table-body">
                        <tr><td colspan="6" style="text-align: center; color: #94a3b8; padding: 20px;">အချက်အလက် မရှိသေးပါ။</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

        <!-- ================= 3. CATALOG MANAGEMENT SECTION ================= -->
        <div id="section-catalog" class="card hidden">
            <h2 style="font-size: 18px; font-weight: bold; color: #4338ca; margin-bottom: 15px;">ကျောင်းအမည်များ ထည့်သွင်းရန်နှင့် စီမံရန်</h2>
            <div style="display: flex; gap: 10px; margin-bottom: 20px;">
                <input type="text" id="new-school-name" placeholder="ကျောင်းအမည် (ဥပမာ - MK Music School)" style="flex: 1;">
                <button onclick="addSchool()" class="btn">ကျောင်းထည့်မည်</button>
            </div>

            <div style="margin-bottom: 25px; background: #f8fafc; padding: 15px; border-radius: 12px; border: 1px solid #e2e8f0;">
                <h3 style="font-size: 15px; font-weight: bold; color: #334155; margin-bottom: 10px;">🏫 လက်ရှိထည့်သွင်းထားသော ကျောင်းများစာရင်း</h3>
                <div style="max-height: 200px; overflow-y: auto;">
                    <table>
                        <thead>
                            <tr>
                                <th>ကျောင်းအမည်</th>
                                <th style="text-align: center; width: 100px;">လုပ်ဆောင်ချက်</th>
                            </tr>
                        </thead>
                        <tbody id="catalog-schools-list-body">
                            <tr><td colspan="2" style="text-align: center; color: #94a3b8;">ကျောင်းစာရင်း မရှိသေးပါ။</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div style="background: #e0e7ff; border: 1px solid #c7d2fe; padding: 15px; border-radius: 12px; display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px;">
                <div>
                    <h4 style="font-weight: bold; color: #3730a3; font-size: 14px;">📊 Excel ဖြင့် စာအုပ်စာရင်းများ စီမံရန်</h4>
                    <p style="font-size: 12px; color: #4338ca;">Excel ဖိုင်ဖြင့် စာအုပ်စာရင်းသွင်းခြင်း (Import) နှင့် ထုတ်ယူခြင်း (Export) ပြုလုပ်နိုင်ပါသည်။</p>
                </div>
                <div style="display: flex; gap: 8px;">
                    <button onclick="exportBooksExcel()" class="btn btn-warning" style="font-size: 13px; background: #059669; color: white;">📤 စာအုပ်စာရင်း Excel ထုတ်ရန်</button>
                    <label class="btn" style="background: #4338ca; font-size: 13px; cursor: pointer; display: inline-block; margin: 0;">
                        📥 Excel ဖြင့် စာအုပ်သွင်းမည် <input type="file" id="import-excel-file" onchange="importBooksExcel(event)" class="hidden" accept=".xlsx, .xls, .csv">
                    </label>
                </div>
            </div>

            <h2 id="form-title" style="font-size: 16px; font-weight: bold; color: #4338ca; margin-bottom: 12px;">သင်တန်းကျောင်းအောက်ရှိ စာအုပ်များ ထည့်သွင်းရန်</h2>
            <input type="hidden" id="edit-book-id" value="">
            <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; margin-bottom: 12px;">
                <select id="book-school-select">
                    <option value="">-- ကျောင်းရွေးချယ်ပါ --</option>
                </select>
                <input type="text" id="book-no" placeholder="Book No. (ဥပမာ - 1, 89)">
                <input type="text" id="book-title" placeholder="စာအုပ်/ပစ္စည်း အမည်">
            </div>
            <div style="margin-bottom: 15px;">
                <input type="number" id="book-price" placeholder="၁ အုပ်/ခု ဈေးနှုန်း (ကျပ်)">
            </div>
            <div style="display: flex; gap: 10px; margin-bottom: 25px;">
                <button id="save-book-btn" onclick="saveBook()" class="btn btn-success">စာအုပ်စာရင်း သွင်းမည်</button>
                <button id="cancel-edit-btn" onclick="resetBookForm()" class="btn hidden" style="background: #64748b;">မလုပ်တော့ပါ</button>
            </div>

            <!-- School Filter for Catalog -->
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; border-top: 1px solid #e2e8f0; padding-top: 20px;">
                <h3 style="font-size: 16px; font-weight: bold; color: #334155;">လက်ရှိထည့်သွင်းထားသော စာအုပ်များ</h3>
                <div style="display: flex; align-items: center; gap: 10px; min-width: 280px;">
                    <label style="margin: 0; white-space: nowrap; font-size: 13px;">ကျောင်းအလိုက် စစ်ဆေးရန်:</label>
                    <select id="catalog-filter-school" onchange="renderCatalogBooks()">
                        <option value="all">-- ကျောင်းအားလုံး --</option>
                    </select>
                </div>
            </div>

            <div style="overflow-x-auto;">
                <table>
                    <thead>
                        <tr>
                            <th>ကျောင်းအမည်</th>
                            <th>No.</th>
                            <th>စာအုပ်/ပစ္စည်း အမည်</th>
                            <th>၁ အုပ်/ခု ဈေးနှုန်း</th>
                            <th style="text-align: center;">လုပ်ဆောင်ချက်</th>
                        </tr>
                    </thead>
                    <tbody id="catalog-book-list">
                        <tr><td colspan="5" style="text-align: center; color: #94a3b8; padding: 20px;">ဤကျောင်းအတွက် စာအုပ်စာရင်း မရှိသေးပါ။</td></tr>
                    </tbody>
                </table>
            </div>
        </div>

    </main>

    <!-- ================= EDIT & PRINT SALE RECORD MODAL ================= -->
    <div id="edit-sale-modal" class="modal hidden">
        <div class="modal-content" style="max-width: 650px;">
            <div class="modal-header">
                <h3 id="modal-title-text" style="font-size: 16px; font-weight: bold;">✏️ Invoice အသေးစိတ် ပြင်ဆင်ရန် / ပရင့်ထုတ်ရန်</h3>
                <button onclick="closeEditSaleModal()" style="background: none; border: none; color: white; font-size: 20px; cursor: pointer;">&times;</button>
            </div>
            <div class="modal-body" style="display: flex; flex-direction: column; gap: 15px;">
                <input type="hidden" id="edit-sale-id">
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                    <div>
                        <label>Invoice နံပါတ်</label>
                        <input type="text" id="edit-sale-invoice-no" readonly style="background: #e2e8f0; font-weight: bold; color: #4338ca;">
                    </div>
                    <div>
                        <label>Customer Name</label>
                        <input type="text" id="edit-sale-customer-name" style="font-weight: bold;">
                    </div>
                </div>

                <div style="border-top: 1px solid #e2e8f0; padding-top: 10px;">
                    <label style="margin-bottom: 6px; font-weight: bold; color: #4338ca;">🛒 ဝယ်ယူထားသော စာရင်းများ (ပြင်ဆင်နိုင်သည်)</label>
                    <div id="edit-sale-items-list" style="max-height: 200px; overflow-y: auto; border: 1px solid #cbd5e1; padding: 10px; border-radius: 8px; background: #f8fafc; margin-bottom: 10px;">
                        <!-- Editable Items list -->
                    </div>
                    <div style="display: flex; justify-content: space-between; font-weight: bold; font-size: 16px; padding: 8px; background: #e0e7ff; border-radius: 8px;">
                        <span>ပြင်ဆင်ပြီး စုစုပေါင်း ကျသင့်ငွေ:</span>
                        <span id="edit-sale-calculated-total" style="color: #4338ca;">0 ကျပ်</span>
                    </div>
                </div>

                <div style="border-top: 1px solid #e2e8f0; padding-top: 15px;">
                    <label>ဘောင်ချာ ဆိုဒ်ရွေးချယ်ရန် (Receipt Size)</label>
                    <select id="edit-receipt-size" onchange="updateEditReceiptStyle()" style="margin-top: 6px;">
                        <option value="a4">Standard A4 / A5 Paper (15 Lines)</option>
                        <option value="thermal-80">Thermal Receipt (80mm)</option>
                        <option value="thermal-58">Thermal Receipt (58mm)</option>
                    </select>
                </div>

                <!-- Printable Preview Inside Edit Modal -->
                <div id="printable-edit-receipt" class="receipt-container-box">
                    <div class="receipt-inner-content">
                        <div style="text-align: center; border-bottom: 1px dashed #cbd5e1; padding-bottom: 8px; margin-bottom: 8px;">
                            <img id="edit-receipt-banner" class="receipt-header-banner" alt="Yoon Header">
                            <div style="display: flex; justify-content: space-between; margin-top: 6px; font-size: 11px; font-weight: bold; color: #334155;">
                                <span id="edit-receipt-inv-display">Invoice: -</span>
                                <span id="edit-receipt-datetime">ရက်စွဲ: -</span>
                            </div>
                            <div id="edit-receipt-customer-display" style="text-align: left; font-size: 11px; font-weight: bold; color: #334155; margin-top: 4px;">Customer Name: -</div>
                        </div>

                        <div id="edit-receipt-items-container" style="margin-bottom: 10px;">
                            <!-- Items injected here -->
                        </div>

                        <div style="display: flex; justify-content: space-between; font-weight: bold; font-size: 14px; margin-top: 10px; border-top: 1px solid #000; padding-top: 6px;">
                            <span>စုစုပေါင်း ကျသင့်ငွေ:</span>
                            <span id="edit-receipt-total" style="color: #000;">0 ကျပ်</span>
                        </div>

                        <div style="text-align: center; font-size: 10px; color: #64748b; margin-top: 10px;">
                            <p>ကျေးဇူးတင်ပါသည်။ ပြန်လည်လာရောက်အားပေးပါရန် ဖိတ်ခေါ်အပ်ပါသည်။ ❤️</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="modal-footer">
                <button onclick="closeEditSaleModal()" class="btn" style="background: #e2e8f0; color: #334155;">ပိတ်မည်</button>
                <button onclick="saveEditedSale()" class="btn btn-success" style="flex: 1;">💾 အပြောင်းအလဲ သိမ်းမည်</button>
                <button onclick="saveReceiptAsJPEG('printable-edit-receipt')" class="btn btn-warning" style="background: #0284c7; color: white;">🖼️ JPEG သိမ်းမည်</button>
                <button onclick="saveAndPrintSaleEdit()" class="btn btn-warning" style="flex: 1;">🖨️ ပရင့်ထုတ်မည်</button>
            </div>
        </div>
    </div>

    <!-- ================= CHECKOUT / RECEIPT MODAL ================= -->
    <div id="checkout-modal" class="modal hidden">
        <div class="modal-content">
            <div class="modal-header">
                <h3 style="font-size: 16px; font-weight: bold;">🖨️ ဘောင်ချာနှင့် ငွေရှင်းခြင်း (Checkout)</h3>
                <button onclick="closeCheckoutModal()" style="background: none; border: none; color: white; font-size: 20px; cursor: pointer;">&times;</button>
            </div>
            
            <div class="modal-body">
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; background: #f1f5f9; padding: 10px; border-radius: 10px;">
                    <div style="flex: 1; margin-right: 10px;">
                        <label style="margin: 0; font-size: 12px;">ဘောင်ချာ ဆိုဒ်ရွေးရန်</label>
                        <select id="receipt-size" onchange="updateReceiptStyle()" style="padding: 6px;">
                            <option value="a4">Standard A4 / A5 Paper (15 Lines)</option>
                            <option value="thermal-80">Thermal Receipt (80mm)</option>
                            <option value="thermal-58">Thermal Receipt (58mm)</option>
                        </select>
                    </div>
                    <div>
                        <label class="btn btn-warning header-change-btn" style="font-size: 12px; padding: 8px 12px; margin: 0; cursor: pointer; display: inline-block;">
                            📂 Heading ပုံစံ ပြောင်းရန်
                            <input type="file" accept="image/*" onchange="changeCustomHeader(event)" class="hidden">
                        </label>
                    </div>
                </div>

                <div id="printable-receipt" class="receipt-container-box">
                    <div class="receipt-inner-content">
                        <div style="text-align: center; border-bottom: 1px dashed #cbd5e1; padding-bottom: 8px; margin-bottom: 8px;">
                            <img id="receipt-banner" class="receipt-header-banner" alt="Yoon Header">
                            <div style="display: flex; justify-content: space-between; margin-top: 6px; font-size: 11px; font-weight: bold; color: #334155;">
                                <span id="receipt-inv-display">Invoice: -</span>
                                <span id="receipt-datetime">ရက်စွဲ: -</span>
                            </div>
                            <div id="receipt-customer-display" style="text-align: left; font-size: 11px; font-weight: bold; color: #334155; margin-top: 4px;">Customer Name: -</div>
                        </div>

                        <div id="receipt-items-container" style="margin-bottom: 10px;">
                            <!-- Items injected here -->
                        </div>

                        <div style="display: flex; justify-content: space-between; font-weight: bold; font-size: 14px; margin-top: 10px; border-top: 1px solid #000; padding-top: 6px;">
                            <span>စုစုပေါင်း ကျသင့်ငွေ:</span>
                            <span id="receipt-total" style="color: #000;">0 ကျပ်</span>
                        </div>

                        <div style="text-align: center; font-size: 10px; color: #64748b; margin-top: 10px;">
                            <p>ကျေးဇူးတင်ပါသည်။ ပြန်လည်လာရောက်အားပေးပါရန် ဖိတ်ခေါ်အပ်ပါသည်။ ❤️</p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="modal-footer">
                <button onclick="closeCheckoutModal()" class="btn" style="background: #e2e8f0; color: #334155; flex: 1;">မလုပ်တော့ပါ</button>
                <button onclick="saveReceiptAsJPEG('printable-receipt')" class="btn btn-warning" style="background: #0284c7; color: white; flex: 1;">🖼️ JPEG သိမ်းမည်</button>
                <button onclick="printReceipt()" class="btn btn-success" style="flex: 1;">🖨️ Print ထုတ်မည်</button>
            </div>
        </div>
    </div>

    <!-- JavaScript Logic with Fixed Firebase Realtime Sync -->
    <script>
        const yoonLogoSrc = "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 300 200' width='300' height='200'><text x='15' y='145' font-family='Pyidaungsu, sans-serif' font-size='130' font-weight='bold' fill='%23d90429' stroke='%23000000' stroke-width='3'>ယွန်း</text></svg>";
        
        let customHeaderSrc = localStorage.getItem('custom_header_banner') || "Heading .JPG";

        document.getElementById('header-logo').src = yoonLogoSrc;
        document.getElementById('receipt-banner').src = customHeaderSrc;
        document.getElementById('edit-receipt-banner').src = customHeaderSrc;

        function changeCustomHeader(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                customHeaderSrc = e.target.result;
                localStorage.setItem('custom_header_banner', customHeaderSrc);
                document.getElementById('receipt-banner').src = customHeaderSrc;
                document.getElementById('edit-receipt-banner').src = customHeaderSrc;
                alert('Heading ပုံစံ အသစ်ကို အောင်မြင်စွာ ပြောင်းလဲလိုက်ပါပြီ။');
            };
            reader.readAsDataURL(file);
        }

        let schools = [];
        let books = [];
        let salesHistory = [];
        let cart = [];
        let activeInvoiceNo = "";

        window.addEventListener('DOMContentLoaded', () => {
            const checkFirebase = setInterval(() => {
                if (window.fbDB) {
                    clearInterval(checkFirebase);
                    initRealtimeSync();
                }
            }, 100);
        });

        function initRealtimeSync() {
            const db = window.fbDB;
            const ref = window.fbRef;
            const onValue = window.fbOnValue;

            // 1. Realtime Listen for Schools
            onValue(ref(db, 'schools'), (snapshot) => {
                const data = snapshot.val();
                schools = [];
                if (data) {
                    if (Array.isArray(data)) {
                        schools = data.filter(item => item !== null && item !== undefined);
                    } else if (typeof data === 'object') {
                        schools = Object.keys(data).map(key => {
                            const val = data[key];
                            return typeof val === 'object' && val !== null ? { id: key, ...val } : { id: key, name: val };
                        });
                    }
                }
                loadSchoolDropdowns();
                renderSchoolsList();
            });

            // 2. Realtime Listen for Books
            onValue(ref(db, 'books'), (snapshot) => {
                const data = snapshot.val();
                books = [];
                if (data) {
                    if (Array.isArray(data)) {
                        books = data.filter(item => item !== null && item !== undefined);
                    } else if (typeof data === 'object') {
                        books = Object.keys(data).map(key => ({ id: key, ...data[key] }));
                    }
                }
                renderCatalogBooks();
                filterPOSBooks();
            });

            // 3. Realtime Listen for Sales History
            onValue(ref(db, 'salesHistory'), (snapshot) => {
                const data = snapshot.val();
                salesHistory = [];
                if (data) {
                    if (Array.isArray(data)) {
                        salesHistory = data.filter(item => item !== null && item !== undefined);
                    } else if (typeof data === 'object') {
                        salesHistory = Object.values(data);
                    }
                }
                generateReport();
            });
        }

        function exportBooksExcel() {
            if (books.length === 0) {
                alert('ထုတ်ယူရန် စာအုပ်စာရင်း မရှိသေးပါ။');
                return;
            }
            let exportData = books.map(b => {
                let school = schools.find(s => s.id === b.schoolId);
                return {
                    "School Name": school ? school.name : "",
                    "Book No": b.bookNo,
                    "Book Title": b.title,
                    "Price": b.price
                };
            });

            let worksheet = XLSX.utils.json_to_sheet(exportData);
            let workbook = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(workbook, worksheet, "Books Catalog");
            XLSX.writeFile(workbook, `Yoon_Copy_Books_${new Date().toISOString().slice(0,10)}.xlsx`);
        }

        function importBooksExcel(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    let data = new Uint8Array(e.target.result);
                    let workbook = XLSX.read(data, {type: 'array'});
                    let firstSheetName = workbook.SheetNames[0];
                    let worksheet = workbook.Sheets[firstSheetName];
                    let rows = XLSX.utils.sheet_to_json(worksheet);

                    if (rows.length === 0) {
                        alert('Excel ဖိုင်ထဲတွင် အချက်အလက် မပါရှိပါ။');
                        return;
                    }

                    rows.forEach(row => {
                        let schoolName = row["School Name"] || row["ကျောင်းအမည်"] || "";
                        let bookNo = String(row["Book No"] || row["No"] || "").trim();
                        let title = String(row["Book Title"] || row["Title"] || row["စာအုပ်အမည်"] || "").trim();
                        let price = parseFloat(row["Price"] || row["ဈေးနှုန်း"] || 0);

                        if (schoolName && bookNo && title) {
                            let school = schools.find(s => s.name && s.name.toLowerCase() === schoolName.toLowerCase().trim());
                            let schoolId = "";
                            if (school) {
                                schoolId = school.id;
                            } else {
                                schoolId = 'school_' + Date.now() + Math.floor(Math.random()*1000);
                                let newSchool = { id: schoolId, name: schoolName.trim() };
                                window.fbSet(window.fbRef(window.fbDB, 'schools/' + schoolId), newSchool);
                            }

                            let bookId = 'book_' + Date.now() + Math.floor(Math.random()*10000);
                            let newBook = { id: bookId, schoolId: schoolId, bookNo: bookNo, title: title, price: price, code: bookNo };
                            window.fbSet(window.fbRef(window.fbDB, 'books/' + bookId), newBook);
                        }
                    });
                    alert('Excel မှ စာအုပ်စာရင်းများ အောင်မြင်စွာ တင်ပြီးပါပြီ။');
                } catch (err) {
                    alert('Excel ဖိုင် ဖတ်ရှုရာတွင် အမှားရှိနေပါသည်။');
                }
                document.getElementById('import-excel-file').value = '';
            };
            reader.readAsArrayBuffer(file);
        }

        function switchTab(tab) {
            document.getElementById('section-pos').classList.add('hidden');
            document.getElementById('section-reports').classList.add('hidden');
            document.getElementById('section-catalog').classList.add('hidden');
            
            document.getElementById('tab-pos').className = '';
            document.getElementById('tab-reports').className = '';
            document.getElementById('tab-catalog').className = '';

            if (tab === 'pos') {
                document.getElementById('section-pos').classList.remove('hidden');
                document.getElementById('tab-pos').className = 'active';
                loadSchoolDropdowns();
            } else if (tab === 'reports') {
                document.getElementById('section-reports').classList.remove('hidden');
                document.getElementById('tab-reports').className = 'active';
                loadSchoolDropdowns();
                generateReport();
            } else if (tab === 'catalog') {
                document.getElementById('section-catalog').classList.remove('hidden');
                document.getElementById('tab-catalog').className = 'active';
                loadSchoolDropdowns();
                renderCatalogBooks();
                renderSchoolsList();
            }
        }

        function generateReport() {
            const schoolFilterId = document.getElementById('report-school-filter').value;
            const invoiceFilter = document.getElementById('filter-invoice-input').value.toLowerCase().trim();
            const tbody = document.getElementById('report-table-body');
            let filteredBills = [...salesHistory];

            if (schoolFilterId !== 'all') {
                filteredBills = filteredBills.filter(bill => {
                    if (bill.isVoided) return false;
                    return bill.items.some(item => {
                        let bObj = books.find(b => b.id === item.id);
                        return bObj && bObj.schoolId === schoolFilterId;
                    });
                });
            }

            if (invoiceFilter) {
                filteredBills = filteredBills.filter(b => 
                    (b.invoiceNo && b.invoiceNo.toLowerCase().includes(invoiceFilter)) ||
                    (b.customerName && b.customerName.toLowerCase().includes(invoiceFilter))
                );
            }

            let schoolStats = {};
            let customerStats = {};

            filteredBills.forEach(bill => {
                if (!bill.isVoided) {
                    if (bill.items && Array.isArray(bill.items)) {
                        bill.items.forEach(item => {
                            let bookObj = books.find(b => b.id === item.id);
                            let schoolName = "အခြား / Free Sale";
                            if (bookObj) {
                                let schoolObj = schools.find(s => s.id === bookObj.schoolId);
                                if (schoolObj) schoolName = schoolObj.name;
                            }
                            if (!schoolStats[schoolName]) {
                                schoolStats[schoolName] = { qty: 0, amount: 0 };
                            }
                            schoolStats[schoolName].qty += item.qty;
                            schoolStats[schoolName].amount += (item.qty * item.price);
                        });
                    }

                    let custName = (bill.customerName && bill.customerName.trim() !== "") ? bill.customerName.trim() : "အမည်မဖော်ပြထားသော ဝယ်သူ";
                    if (!customerStats[custName]) {
                        customerStats[custName] = { billsCount: 0, amount: 0 };
                    }
                    customerStats[custName].billsCount += 1;
                    customerStats[custName].amount += bill.total;
                }
            });

            let schoolTbody = document.getElementById('school-sales-summary-body');
            let schoolHtml = '';
            let schoolKeys = Object.keys(schoolStats);
            if (schoolKeys.length === 0) {
                schoolHtml = `<tr><td colspan="3" style="text-align: center; color: #94a3b8; padding: 12px;">ကျောင်းအလိုက် အရောင်းစာရင်း မရှိသေးပါ။</td></tr>`;
            } else {
                schoolKeys.forEach(sName => {
                    schoolHtml += `
                        <tr>
                            <td style="font-weight: bold; color: #4338ca;">${sName}</td>
                            <td style="text-align: center; font-weight: bold;">${schoolStats[sName].qty.toLocaleString()} အုပ်/ခု</td>
                            <td style="text-align: right; font-weight: bold; color: #059669;">${schoolStats[sName].amount.toLocaleString()} ကျပ်</td>
                        </tr>
                    `;
                });
            }
            schoolTbody.innerHTML = schoolHtml;

            let customerTbody = document.getElementById('customer-sales-summary-body');
            let customerHtml = '';
            let customerKeys = Object.keys(customerStats);
            if (customerKeys.length === 0) {
                customerHtml = `<tr><td colspan="3" style="text-align: center; color: #94a3b8; padding: 12px;">Customer အလိုက် အရောင်းစာရင်း မရှိသေးပါ။</td></tr>`;
            } else {
                customerKeys.forEach(cName => {
                    customerHtml += `
                        <tr>
                            <td style="font-weight: bold; color: #334155;">${cName}</td>
                            <td style="text-align: center; font-weight: bold;">${customerStats[cName].billsCount.toLocaleString()} စောင်</td>
                            <td style="text-align: right; font-weight: bold; color: #059669;">${customerStats[cName].amount.toLocaleString()} ကျပ်</td>
                        </tr>
                    `;
                });
            }
            customerTbody.innerHTML = customerHtml;

            let html = '';
            filteredBills.forEach(bill => {
                const isVoided = bill.isVoided || false;
                let statusBadge = isVoided ? 
                    `<span style="background: #fee2e2; color: #991b1b; padding: 3px 8px; border-radius: 6px; font-size: 11px; font-weight: bold;">ပယ်ဖျက်ပြီး (Void)</span>` : 
                    `<span style="background: #d1fae5; color: #065f46; padding: 3px 8px; border-radius: 6px; font-size: 11px; font-weight: bold;">ပုံမှန်</span>`;

                html += `
                    <tr>
                        <td>${bill.timestamp}</td>
                        <td style="font-weight: bold;"><a href="#" onclick="openEditSaleModal('${bill.id}'); return false;" style="color: #4338ca; text-decoration: underline;">${bill.invoiceNo || 'INV-OLD'}</a></td>
                        <td>${bill.customerName || '-'}</td>
                        <td style="text-align: right; font-weight: bold; color: ${isVoided ? '#94a3b8' : '#4338ca'};">${isVoided ? '-' + bill.total.toLocaleString() + ' ကျပ် (Void)' : bill.total.toLocaleString() + ' ကျပ်'}</td>
                        <td style="text-align: center;">${statusBadge}</td>
                        <td style="text-align: center;">
                            <button onclick="openEditSaleModal('${bill.id}')" class="btn" style="padding: 4px 8px; font-size: 11px; background: #4338ca;">ပြင်ရန် / ကြည့်ရန်</button>
                            ${!isVoided ? `<button onclick="voidSaleRecord('${bill.id}')" class="btn btn-danger" style="padding: 4px 8px; font-size: 11px; margin-left: 4px;">ဖျက်သိမ်း (Void)</button>` : ''}
                        </td>
                    </tr>
                `;
            });

            if (filteredBills.length === 0) {
                html = `<tr><td colspan="6" style="text-align: center; color: #94a3b8; padding: 20px;">ဤအချက်အလက်အတွက် အရောင်းစာရင်း မရှိပါ။</td></tr>`;
            }

            tbody.innerHTML = html;
        }

        function openEditSaleModal(saleId) {
            const sale = salesHistory.find(s => s.id === saleId);
            if (!sale) return;

            document.getElementById('edit-sale-id').value = sale.id;
            document.getElementById('edit-sale-invoice-no').value = sale.invoiceNo || 'INV-OLD';
            document.getElementById('edit-sale-customer-name').value = sale.customerName || '';
            document.getElementById('edit-receipt-inv-display').innerText = `Invoice: ${sale.invoiceNo || 'INV-OLD'}`;
            document.getElementById('edit-receipt-datetime').innerText = `ရက်စွဲ: ${sale.timestamp}`;
            document.getElementById('edit-receipt-customer-display').innerText = `Customer Name: ${sale.customerName || '-'}`;

            renderEditableSaleItems(sale.items);
            updateEditReceiptPreview(sale.items, sale.total);

            document.getElementById('edit-sale-modal').classList.remove('hidden');
        }

        function renderEditableSaleItems(items) {
            const container = document.getElementById('edit-sale-items-list');
            let html = '';
            items.forEach((item, index) => {
                let itemCode = item.code || '-';
                let itemTitle = item.title || '';
                html += `
                    <div style="display: flex; gap: 8px; align-items: center; margin-bottom: 6px; background: white; padding: 6px; border-radius: 6px; border: 1px solid #e2e8f0;">
                        <input type="text" value="${itemCode}" id="edit-item-code-${index}" style="width: 70px; padding: 4px; font-size: 13px; text-align: center;" placeholder="Code">
                        <input type="text" value="${itemTitle}" id="edit-item-title-${index}" style="flex: 2; padding: 4px; font-size: 13px;" placeholder="အမည်">
                        <input type="number" value="${item.price}" id="edit-item-price-${index}" oninput="calculateEditModalTotal()" style="flex: 1; padding: 4px; font-size: 13px;" placeholder="ဈေးနှုန်း">
                        <input type="number" value="${item.qty}" id="edit-item-qty-${index}" min="1" oninput="calculateEditModalTotal()" style="width: 50px; padding: 4px; font-size: 13px; text-align: center;" placeholder="ခုရေ">
                        <button onclick="removeEditItemRow(${index})" class="btn btn-danger" style="padding: 4px 8px; font-size: 11px;">❌</button>
                    </div>
                `;
            });
            container.innerHTML = html;
            calculateEditModalTotal();
        }

        function removeEditItemRow(index) {
            const saleId = document.getElementById('edit-sale-id').value;
            const sale = salesHistory.find(s => s.id === saleId);
            if (sale && sale.items.length > 1) {
                sale.items.splice(index, 1);
                renderEditableSaleItems(sale.items);
                updateEditReceiptPreview(sale.items, calculateCurrentEditTotalValue());
            } else {
                alert('ဘောင်ချာတွင် အနည်းဆုံး ပစ္စည်းတစ်ခု ပါရှိရပါမည်။');
            }
        }

        function calculateCurrentEditTotalValue() {
            const container = document.getElementById('edit-sale-items-list');
            const rows = container.children;
            let total = 0;
            for (let i = 0; i < rows.length; i++) {
                const price = parseFloat(document.getElementById(`edit-item-price-${i}`).value) || 0;
                const qty = parseInt(document.getElementById(`edit-item-qty-${i}`).value) || 0;
                total += (price * qty);
            }
            return total;
        }

        function calculateEditModalTotal() {
            const total = calculateCurrentEditTotalValue();
            document.getElementById('edit-sale-calculated-total').innerText = `${total.toLocaleString()} ကျပ်`;
            
            const container = document.getElementById('edit-sale-items-list');
            const rows = container.children;
            let tempItems = [];
            for (let i = 0; i < rows.length; i++) {
                tempItems.push({
                    code: document.getElementById(`edit-item-code-${i}`).value,
                    title: document.getElementById(`edit-item-title-${i}`).value,
                    price: parseFloat(document.getElementById(`edit-item-price-${i}`).value) || 0,
                    qty: parseInt(document.getElementById(`edit-item-qty-${i}`).value) || 0
                });
            }
            updateEditReceiptPreview(tempItems, total);
        }

        function updateEditReceiptPreview(items, total) {
            document.getElementById('edit-receipt-items-container').innerHTML = buildReceiptTableHTML(items);
            document.getElementById('edit-receipt-total').innerText = `${total.toLocaleString()} ကျပ်`;
            const custName = document.getElementById('edit-sale-customer-name').value;
            document.getElementById('edit-receipt-customer-display').innerText = `Customer Name: ${custName || '-'}`;
        }

        function saveEditedSale() {
            const saleId = document.getElementById('edit-sale-id').value;
            const sale = salesHistory.find(s => s.id === saleId);
            if (!sale) return;

            const custName = document.getElementById('edit-sale-customer-name').value.trim();
            const container = document.getElementById('edit-sale-items-list');
            const rows = container.children;
            let newItems = [];
            let newTotal = 0;

            for (let i = 0; i < rows.length; i++) {
                const code = document.getElementById(`edit-item-code-${i}`).value;
                const title = document.getElementById(`edit-item-title-${i}`).value;
                const price = parseFloat(document.getElementById(`edit-item-price-${i}`).value) || 0;
                const qty = parseInt(document.getElementById(`edit-item-qty-${i}`).value) || 0;
                newItems.push({ code, title, price, qty });
                newTotal += (price * qty);
            }

            sale.customerName = custName;
            sale.items = newItems;
            sale.total = newTotal;

            window.fbSet(window.fbRef(window.fbDB, 'salesHistory'), salesHistory);
            generateReport();
            alert('Invoice အချက်အလက်များကို အောင်မြင်စွာ ပြင်ဆင်ပြီးပါပြီ။');
            closeEditSaleModal();
        }

        function closeEditSaleModal() {
            document.getElementById('edit-sale-modal').classList.add('hidden');
        }

        function buildReceiptTableHTML(items) {
            let rowsHtml = '';
            items.forEach((item, index) => {
                const subtotal = item.qty * item.price;
                const codeVal = item.code || '-';
                rowsHtml += `
                    <tr>
                        <td style="text-align: center; width: 30px;">${index + 1}</td>
                        <td style="text-align: center; width: 50px; font-weight: bold; color: #4338ca;">${codeVal}</td>
                        <td>${item.title}</td>
                        <td style="text-align: center; width: 40px;">${item.qty}</td>
                        <td style="text-align: right; width: 70px;">${item.price.toLocaleString()}</td>
                        <td style="text-align: right; width: 80px;">${subtotal.toLocaleString()}</td>
                    </tr>
                `;
            });

            const totalLinesNeeded = 15;
            for (let i = items.length; i < totalLinesNeeded; i++) {
                rowsHtml += `
                    <tr>
                        <td style="text-align: center; color: transparent;">${i + 1}</td>
                        <td style="color: transparent;">-</td>
                        <td style="color: transparent;">-</td>
                        <td style="color: transparent;">-</td>
                        <td style="color: transparent;">-</td>
                        <td style="color: transparent;">-</td>
                    </tr>
                `;
            }

            return `
                <table class="receipt-table">
                    <thead>
                        <tr>
                            <th style="text-align: center; width: 30px;">စဉ်</th>
                            <th style="text-align: center; width: 50px;">Code</th>
                            <th>အကြောင်းအရာ</th>
                            <th style="text-align: center; width: 40px;">ခုရေ</th>
                            <th style="text-align: right; width: 70px;">နှုန်း</th>
                            <th style="text-align: right; width: 80px;">သင့်ငွေ</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${rowsHtml}
                    </tbody>
                </table>
            `;
        }

        function updateEditReceiptStyle() {
            const size = document.getElementById('edit-receipt-size').value;
            const receiptEl = document.getElementById('printable-edit-receipt');
            
            if (size === 'thermal-80') {
                receiptEl.style.maxWidth = '300px';
                receiptEl.style.margin = '0 auto';
            } else if (size === 'thermal-58') {
                receiptEl.style.maxWidth = '220px';
                receiptEl.style.margin = '0 auto';
            } else if (size === 'a4') {
                receiptEl.style.maxWidth = '100%';
            }
        }

        function saveAndPrintSaleEdit() {
            saveEditedSale();
            document.body.classList.remove('print-checkout');
            document.body.classList.add('print-report');
            window.print();
        }

        function voidSaleRecord(saleId) {
            if (confirm('ဤ Invoice ကို ဖျက်သိမ်းရန် (Void) သေချာပါသလား?')) {
                const saleIndex = salesHistory.findIndex(s => s.id === saleId);
                if (saleIndex !== -1) {
                    salesHistory[saleIndex].isVoided = true;
                    window.fbSet(window.fbRef(window.fbDB, 'salesHistory'), salesHistory);
                    generateReport();
                }
            }
        }

        function addSchool() {
            const nameInput = document.getElementById('new-school-name');
            const name = nameInput.value.trim();
            if (!name) {
                alert('ကျောင်းအမည် ထည့်သွင်းပါ။');
                return;
            }
            const schoolId = 'school_' + Date.now();
            const newSchool = { id: schoolId, name: name };
            
            window.fbSet(window.fbRef(window.fbDB, 'schools/' + schoolId), newSchool).then(() => {
                nameInput.value = '';
                alert('ကျောင်းအမည် အောင်မြင်စွာ ထည့်သွင်းပြီးပါပြီ။');
            }).catch(err => {
                alert('ကျောင်းထည့်သွင်းရာတွင် အမှားရှိနေပါသည်။');
            });
        }

        function renderSchoolsList() {
            const tbody = document.getElementById('catalog-schools-list-body');
            if (!tbody) return;

            if (schools.length === 0) {
                tbody.innerHTML = `<tr><td colspan="2" style="text-align: center; color: #94a3b8; padding: 10px;">ကျောင်းစာရင်း မရှိသေးပါ။</td></tr>`;
                return;
            }

            let html = '';
            schools.forEach(s => {
                if(s && s.name) {
                    html += `
                        <tr>
                            <td style="font-weight: bold; color: #334155;">${s.name}</td>
                            <td style="text-align: center;">
                                <button onclick="deleteSchool('${s.id}')" class="btn btn-danger" style="padding: 4px 8px; font-size: 11px;">ဖျက်မည်</button>
                            </td>
                        </tr>
                    `;
                }
            });
            tbody.innerHTML = html;
        }

        function deleteSchool(schoolId) {
            if (confirm('ဤကျောင်းကို ဖျက်ရန် သေချာပါသလား?')) {
                window.fbSet(window.fbRef(window.fbDB, 'schools/' + schoolId), null);
                books.forEach(b => {
                    if (b.schoolId === schoolId) {
                        window.fbSet(window.fbRef(window.fbDB, 'books/' + b.id), null);
                    }
                });
                alert('ကျောင်းကို အောင်မြင်စွာ ဖျက်သိမ်းပြီးပါပြီ။');
            }
        }

        function loadSchoolDropdowns() {
            const posSelect = document.getElementById('pos-school-select');
            const bookSelect = document.getElementById('book-school-select');
            const catalogFilterSelect = document.getElementById('catalog-filter-school');
            const reportSchoolFilter = document.getElementById('report-school-filter');
            
            let options = '<option value="">-- ကျောင်းရွေးချယ်ပါ --</option>';
            let catalogOptions = '<option value="all">-- ကျောင်းအားလုံး --</option>';

            schools.forEach(s => {
                if(s && s.name && s.id) {
                    options += `<option value="${s.id}">${s.name}</option>`;
                    catalogOptions += `<option value="${s.id}">${s.name}</option>`;
                }
            });

            if(posSelect) posSelect.innerHTML = options;
            if(bookSelect) bookSelect.innerHTML = options;
            if(catalogFilterSelect) catalogFilterSelect.innerHTML = catalogOptions;
            if(reportSchoolFilter) reportSchoolFilter.innerHTML = catalogOptions;
        }

        function saveBook() {
            const editId = document.getElementById('edit-book-id').value;
            const schoolId = document.getElementById('book-school-select').value;
            const bookNo = document.getElementById('book-no').value.trim();
            const title = document.getElementById('book-title').value.trim();
            const price = parseFloat(document.getElementById('book-price').value) || 0;

            if (!schoolId || !bookNo || !title) {
                alert('ကျောင်း၊ No. နှင့် စာအုပ်အမည်ကို အပြည့်အစုံ ဖြည့်စွက်ပါ။');
                return;
            }

            const bookId = editId ? editId : ('book_' + Date.now());
            const bookData = { id: bookId, schoolId, bookNo, title, price, code: bookNo };

            window.fbSet(window.fbRef(window.fbDB, 'books/' + bookId), bookData).then(() => {
                resetBookForm();
                alert('စာအုပ်စာရင်း အောင်မြင်စွာ သိမ်းဆည်းပြီးပါပြီ။');
            });
        }

        function editBook(id) {
            const book = books.find(b => b.id === id);
            if (!book) return;

            document.getElementById('edit-book-id').value = book.id;
            document.getElementById('book-school-select').value = book.schoolId;
            document.getElementById('book-no').value = book.bookNo;
            document.getElementById('book-title').value = book.title;
            document.getElementById('book-price').value = book.price || 0;

            document.getElementById('form-title').innerText = 'စာအုပ်အချက်အလက် ပြင်ဆင်ရန် (Edit Book)';
            document.getElementById('save-book-btn').innerText = 'ပြင်ဆင်မှု သိမ်းဆည်းမည် (Update)';
            document.getElementById('cancel-edit-btn').classList.remove('hidden');

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function resetBookForm() {
            document.getElementById('edit-book-id').value = '';
            document.getElementById('book-school-select').value = '';
            document.getElementById('book-no').value = '';
            document.getElementById('book-title').value = '';
            document.getElementById('book-price').value = '';

            document.getElementById('form-title').innerText = 'သင်တန်းကျောင်းအောက်ရှိ စာအုပ်များ ထည့်သွင်းရန်';
            document.getElementById('save-book-btn').innerText = 'စာအုပ်စာရင်း သွင်းမည်';
            document.getElementById('cancel-edit-btn').classList.add('hidden');
        }

        function renderCatalogBooks() {
            const tbody = document.getElementById('catalog-book-list');
            const selectedSchoolId = document.getElementById('catalog-filter-school') ? document.getElementById('catalog-filter-school').value : 'all';

            if (!tbody) return;

            let filteredBooks = books;
            if (selectedSchoolId !== 'all' && selectedSchoolId !== '') {
                filteredBooks = books.filter(b => b && b.schoolId === selectedSchoolId);
            }

            if (filteredBooks.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" style="text-align: center; color: #94a3b8; padding: 20px;">ဤကျောင်းအတွက် စာအုပ်စာရင်း မရှိသေးပါ။</td></tr>`;
                return;
            }

            let html = '';
            filteredBooks.forEach((b) => {
                if (b && b.title) {
                    const school = schools.find(s => s && s.id === b.schoolId);
                    const schoolName = school ? school.name : 'Unknown';
                    const price = b.price || 0;

                    html += `
                        <tr>
                            <td>${schoolName}</td>
                            <td style="font-weight: bold; color: #4338ca;">${b.bookNo}</td>
                            <td>${b.title}</td>
                            <td style="font-weight: bold; color: #4338ca;">${price.toLocaleString()} ကျပ်</td>
                            <td style="text-align: center;">
                                <button onclick="editBook('${b.id}')" class="btn btn-warning" style="padding: 4px 8px; font-size: 11px;">ပြင်မည်</button>
                                <button onclick="deleteBook('${b.id}')" class="btn btn-danger" style="padding: 4px 8px; font-size: 11px;">ဖျက်မည်</button>
                            </td>
                        </tr>
                    `;
                }
            });
            tbody.innerHTML = html;
        }

        function deleteBook(id) {
            if (confirm('ဤစာအုပ်ကို ဖျက်ရန် သေချာပါသလား?')) {
                window.fbSet(window.fbRef(window.fbDB, 'books/' + id), null);
            }
        }

        function filterPOSBooks() {
            const schoolId = document.getElementById('pos-school-select').value;
            const keyword = document.getElementById('pos-search-input').value.toLowerCase().trim();
            const tbody = document.getElementById('pos-book-list');

            if (!schoolId) {
                tbody.innerHTML = `<tr><td colspan="5" style="text-align: center; color: #94a3b8; padding: 20px;">ကျေးဇူးပြု၍ ကျောင်းတစ်ကျောင်း ရွေးချယ်ပါ။</td></tr>`;
                return;
            }

            let filtered = books.filter(b => b && b.schoolId === schoolId);

            if (keyword) {
                filtered = filtered.filter(b => 
                    (b.bookNo && b.bookNo.toLowerCase().includes(keyword)) || 
                    (b.title && b.title.toLowerCase().includes(keyword))
                );
            }

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" style="text-align: center; color: #94a3b8; padding: 20px;">ရှာမတွေ့ပါ။ (သို့မဟုတ်) စာအုပ်စာရင်း မရှိသေးပါ။</td></tr>`;
                return;
            }

            let html = '';
            filtered.forEach(b => {
                const price = b.price || 0;
                html += `
                    <tr>
                        <td style="font-weight: bold; color: #4338ca;">${b.bookNo}</td>
                        <td>${b.title}</td>
                        <td style="text-align: right; font-weight: bold; color: #4338ca;">${price.toLocaleString()} ကျပ်</td>
                        <td style="text-align: center;">
                            <input type="number" id="qty-${b.id}" value="1" min="1" style="width: 60px; text-align: center; padding: 4px;">
                        </td>
                        <td style="text-align: center;">
                            <button onclick="addWithQty('${b.id}', ${price})" class="btn" style="padding: 6px 12px; font-size: 12px;">ထည့်မည် (+)</button>
                        </td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        function addWithQty(bookId, price) {
            const qtyInput = document.getElementById(`qty-${bookId}`);
            const qty = parseInt(qtyInput.value) || 1;
            const book = books.find(b => b.id === bookId);
            const existingCartItem = cart.find(item => item.id === bookId);

            if (existingCartItem) {
                existingCartItem.qty += qty;
            } else {
                cart.push({
                    id: book.id,
                    code: book.bookNo,
                    title: book.title,
                    price: price,
                    qty: qty
                });
            }
            renderCart();
            qtyInput.value = 1;
        }

        function addFreeItemToCart() {
            const title = document.getElementById('free-book-title').value.trim();
            const price = parseFloat(document.getElementById('free-book-price').value) || 0;
            const qty = parseInt(document.getElementById('free-book-qty').value) || 1;

            if (!title || price <= 0) {
                alert('ကျေးဇူးပြု၍ ပစ္စည်းအမည်နှင့် မှန်ကန်သော ဈေးနှုန်းကို ဖြည့်စွက်ပါ။');
                return;
            }

            cart.push({
                id: 'free_' + Date.now(),
                code: '-',
                title: title,
                price: price,
                qty: qty
            });

            renderCart();
            document.getElementById('free-book-title').value = '';
            document.getElementById('free-book-price').value = '';
            document.getElementById('free-book-qty').value = '1';
        }

        function renderCart() {
            const container = document.getElementById('cart-items');
            const totalEl = document.getElementById('cart-total');
            if(!container) return;

            if (cart.length === 0) {
                container.innerHTML = `<p style="color: #94a3b8; font-size: 13px; text-align: center; padding: 30px 0;">စာအုပ် ရွေးချယ်ထားခြင်း မရှိသေးပါ။</p>`;
                totalEl.innerText = `0 ကျပ်`;
                return;
            }

            let html = '';
            let total = 0;

            cart.forEach((item, index) => {
                const subtotal = item.price * item.qty;
                total += subtotal;

                html += `
                    <div style="background: #f8fafc; border: 1px solid #e2e8f0; padding: 10px; border-radius: 10px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center;">
                        <div>
                            <p style="font-weight: bold; font-size: 13px; color: #334155;">[Code: ${item.code || '-'}] ${item.title}</p>
                            <p style="font-size: 11px; color: #64748b;">${item.price.toLocaleString()} x ${item.qty} = <span style="font-weight: bold; color: #4338ca;">${subtotal.toLocaleString()} ကျပ်</span></p>
                        </div>
                        <div style="display: flex; gap: 4px; align-items: center;">
                            <button onclick="changeQty(${index}, -1)" style="background: #e2e8f0; border: none; width: 24px; height: 24px; border-radius: 6px; font-weight: bold; cursor: pointer;">-</button>
                            <span style="font-size: 12px; font-weight: bold; width: 20px; text-align: center;">${item.qty}</span>
                            <button onclick="changeQty(${index}, 1)" style="background: #e2e8f0; border: none; width: 24px; height: 24px; border-radius: 6px; font-weight: bold; cursor: pointer;">+</button>
                            <button onclick="removeFromCart(${index})" class="btn btn-danger" style="padding: 2px 6px; font-size: 11px; margin-left: 6px;" title="ဖျက်မည်">❌</button>
                        </div>
                    </div>
                `;
            });

            container.innerHTML = html;
            totalEl.innerText = `${total.toLocaleString()} ကျပ်`;
        }

        function changeQty(index, delta) {
            cart[index].qty += delta;
            if (cart[index].qty <= 0) {
                cart.splice(index, 1);
            }
            renderCart();
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            renderCart();
        }

        let currentCheckoutTotal = 0;

        function openCheckoutModal() {
            if (cart.length === 0) {
                alert('ဝယ်ယူမည့် စာအုပ်များ မရှိသေးပါ။');
                return;
            }

            currentCheckoutTotal = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
            
            const now = new Date();
            activeInvoiceNo = `INV-${now.getFullYear()}${String(now.getMonth()+1).padStart(2,'0')}${String(now.getDate()).padStart(2,'0')}-${Math.floor(1000 + Math.random() * 9000)}`;

            const customerName = document.getElementById('pos-customer-input').value.trim() || 'General Customer';

            document.getElementById('receipt-inv-display').innerText = `Invoice: ${activeInvoiceNo}`;
            document.getElementById('receipt-datetime').innerText = `ရက်စွဲ: ${now.toLocaleString()}`;
            document.getElementById('receipt-customer-display').innerText = `Customer Name: ${customerName}`;
            
            document.getElementById('receipt-items-container').innerHTML = buildReceiptTableHTML(cart);
            document.getElementById('receipt-total').innerText = `${currentCheckoutTotal.toLocaleString()} ကျပ်`;

            document.getElementById('checkout-modal').classList.remove('hidden');
        }

        function closeCheckoutModal() {
            document.getElementById('checkout-modal').classList.add('hidden');
        }

        function updateReceiptStyle() {
            const size = document.getElementById('receipt-size').value;
            const receiptEl = document.getElementById('printable-receipt');
            
            if (size === 'thermal-80') {
                receiptEl.style.maxWidth = '300px';
                receiptEl.style.margin = '0 auto';
            } else if (size === 'thermal-58') {
                receiptEl.style.maxWidth = '220px';
                receiptEl.style.margin = '0 auto';
            } else if (size === 'a4') {
                receiptEl.style.maxWidth = '100%';
            }
        }

        function printReceipt() {
            let total = currentCheckoutTotal;
            
            const now = new Date();
            const dateStr = now.toISOString().slice(0, 10);
            const timestamp = now.toLocaleString();
            const customerName = document.getElementById('pos-customer-input').value.trim() || 'General Customer';

            const saleId = 'sale_' + Date.now();
            const billRecord = {
                id: saleId,
                invoiceNo: activeInvoiceNo,
                customerName: customerName,
                dateStr: dateStr,
                timestamp: timestamp,
                items: [...cart],
                total: total,
                isVoided: false
            };

            window.fbSet(window.fbRef(window.fbDB, 'salesHistory/' + saleId), billRecord);

            document.body.classList.remove('print-report');
            document.body.classList.add('print-checkout');
            window.print();

            cart = [];
            document.getElementById('pos-customer-input').value = '';
            renderCart();
            closeCheckoutModal();
        }

        function saveReceiptAsJPEG(elementId) {
            const receiptElement = document.getElementById(elementId);
            if (!receiptElement) return;

            html2canvas(receiptElement, { scale: 2, useCORS: true }).then(canvas => {
                const link = document.createElement('a');
                link.download = `${activeInvoiceNo || 'Yoon_Voucher'}.jpg`;
                link.href = canvas.toDataURL('image/jpeg', 1.0);
                link.click();
            }).catch(err => {
                alert('ဘောင်ချာကို JPEG ဖိုင်အဖြစ် သိမ်းဆည်းရာတွင် အမှားရှိနေပါသည်။');
            });
        }
    </script>
</body>
</html>
