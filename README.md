<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PT TUNAS MANDIRI PERKASA</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        :root {
            --primary-color: #5D5CDE;
            --dark-bg: #181818;
        }

        .dark {
            color-scheme: dark;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            transition: background-color 0.3s, color 0.3s;
        }

        body.dark {
            background-color: var(--dark-bg);
            color: #f3f4f6;
        }

        body:not(.dark) {
            background-color: #ffffff;
            color: #1f2937;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 1rem;
        }

        th, td {
            padding: 0.5rem;
            text-align: left;
            border-bottom: 1px solid rgba(160, 160, 160, 0.3);
        }

        .status-badge {
            padding: 0.25rem 0.5rem;
            border-radius: 0.25rem;
            font-size: 0.75rem;
            font-weight: 500;
        }

        .status-badge.in {
            background-color: rgba(5, 150, 105, 0.2);
            color: rgb(5, 150, 105);
        }

        .status-badge.out {
            background-color: rgba(220, 38, 38, 0.2);
            color: rgb(220, 38, 38);
        }

        .status-badge.shipping {
            background-color: rgba(59, 130, 246, 0.2);
            color: rgb(59, 130, 246);
        }

        .status-badge.return {
            background-color: rgba(245, 158, 11, 0.2);
            color: rgb(245, 158, 11);
        }

        .dark .status-badge {
            color: white;
        }

        .input-group {
            margin-bottom: 1rem;
        }

        .btn {
            cursor: pointer;
            padding: 0.5rem 1rem;
            border-radius: 0.375rem;
            font-weight: 500;
            transition: background-color 0.2s, transform 0.1s;
        }

        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }

        .btn-secondary {
            background-color: rgba(160, 160, 160, 0.2);
            color: inherit;
        }

        .responsive-table {
            overflow-x: auto;
            width: 100%;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="flex justify-between items-center mb-8">
            <h1 class="text-2xl font-bold text-center md:text-left">PT TUNAS MANDIRI PERKASA</h1>
            <button id="theme-toggle" class="btn btn-secondary">Toggle Theme</button>
        </div>

        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mb-8">
            <h2 class="text-lg font-semibold mb-4">Tambah Transaksi</h2>
            <form id="form" class="grid grid-cols-1 md:grid-cols-4 gap-4">
                <div>
                    <label for="itemName" class="block text-sm font-medium mb-1">Nama Barang</label>
                    <input type="text" id="itemName" placeholder="Nama Barang" required
                        class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-base dark:bg-gray-700">
                </div>
                <div>
                    <label for="itemQuantity" class="block text-sm font-medium mb-1">Jumlah</label>
                    <input type="number" id="itemQuantity" placeholder="Jumlah" min="1" required
                        class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-base dark:bg-gray-700">
                </div>
                <div>
                    <label for="transactionType" class="block text-sm font-medium mb-1">Jenis Transaksi</label>
                    <select id="transactionType" 
                        class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-base dark:bg-gray-700">
                        <option value="in">Barang Masuk</option>
                        <option value="out">Barang Keluar</option>
                        <option value="shipping">Pengiriman</option>
                        <option value="return">Pengembalian</option>
                    </select>
                </div>
                <div>
                    <label for="destinationAddress" class="block text-sm font-medium mb-1">Alamat Tujuan</label>
                    <input type="text" id="destinationAddress" placeholder="Alamat Tujuan" style="display:none;"
                        class="w-full px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-base dark:bg-gray-700">
                </div>
                <div class="flex items-end">
                    <button type="submit" class="btn btn-primary w-full">Proses</button>
                </div>
            </form>
        </div>
        
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6">
                <h2 class="text-lg font-semibold mb-4">Laporan Stok</h2>
                <div class="flex justify-between items-center mb-4">
                    <input type="date" id="reportDate" 
                        class="px-3 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-sm dark:bg-gray-700">
                    <button id="reportButton" class="btn btn-primary">Lihat Laporan Stok</button>
                </div>
                <div class="responsive-table">
                    <table class="w-full" id="stockReport">
                        <thead>
                            <tr>
                                <th>Nama Barang</th>
                                <th>Stok Akhir</th>
                            </tr>
                        </thead>
                        <tbody id="stockList"></tbody>
                    </table>
                </div>
                <div id="emptyStockMessage" class="hidden text-center py-4 text-gray-500 dark:text-gray-400">
                    Belum ada data stok. Tambahkan transaksi terlebih dahulu.
                </div>
            </div>

            <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6">
                <h2 class="text-lg font-semibold mb-4">Daftar Transaksi</h2>
                <button id="transactionReportButton" class="btn btn-primary mb-4">Lihat Semua Transaksi</button>
                <div class="responsive-table">
                    <table class="w-full">
                        <thead>
                            <tr>
                                <th>Nama Barang</th>
                                <th>Jumlah</th>
                                <th>Jenis Transaksi</th>
                                <th>Tanggal</th>
                                <th>Alamat Tujuan</th>
                            </tr>
                        </thead>
                        <tbody id="transactionList"></tbody>
                    </table>
                </div>
                <div id="emptyTransactionMessage" class="hidden text-center py-4 text-gray-500 dark:text-gray-400">
                    Belum ada transaksi. Tambahkan transaksi terlebih dahulu.
                </div>
            </div>
        </div>

        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 mt-8">
            <h2 class="text-lg font-semibold mb-4">Laporan Lengkap Transaksi</h2>
            <div class="responsive-table">
                <table class="w-full" id="fullTransactionReport">
                    <thead>
                        <tr>
                            <th>Nama Barang</th>
                            <th>Jumlah</th>
                            <th>Jenis Transaksi</th>
                            <th>Tanggal</th>
                            <th>Alamat Tujuan</th>
                        </tr>
                    </thead>
                    <tbody id="fullTransactionList"></tbody>
                </table>
            </div>
            <div id="emptyFullTransactionMessage" class="hidden text-center py-4 text-gray-500 dark:text-gray-400">
                Belum ada transaksi. Tambahkan transaksi terlebih dahulu.
            </div>
        </div>
    </div>

    <script>
        // Dark mode detection and toggle
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            document.body.classList.add('dark');
        }
        
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            if (event.matches) {
                document.body.classList.add('dark');
            } else {
                document.body.classList.remove('dark');
            }
        });

        document.getElementById('theme-toggle').addEventListener('click', () => {
            document.body.classList.toggle('dark');
        });

        // Data storage
        let transactions = JSON.parse(localStorage.getItem('transactions')) || [];
        
        // DOM elements
        const form = document.getElementById('form');
        const itemNameInput = document.getElementById('itemName');
        const itemQuantityInput = document.getElementById('itemQuantity');
        const transactionTypeSelect = document.getElementById('transactionType');
        const destinationAddressInput = document.getElementById('destinationAddress');
        const transactionList = document.getElementById('transactionList');
        const stockList = document.getElementById('stockList');
        const fullTransactionList = document.getElementById('fullTransactionList');
        const reportDateInput = document.getElementById('reportDate');
        const reportButton = document.getElementById('reportButton');
        const transactionReportButton = document.getElementById('transactionReportButton');
        const emptyStockMessage = document.getElementById('emptyStockMessage');
        const emptyTransactionMessage = document.getElementById('emptyTransactionMessage');
        const emptyFullTransactionMessage = document.getElementById('emptyFullTransactionMessage');

        // Set today's date as default for report date
        const today = new Date().toISOString().split('T')[0];
        reportDateInput.value = today;

        // Translation mapping for transaction types
        const transactionLabels = {
            'in': 'Barang Masuk',
            'out': 'Barang Keluar',
            'shipping': 'Pengiriman',
            'return': 'Pengembalian'
        };

        // Form submit handler
        form.addEventListener('submit', (e) => {
            e.preventDefault();
            
            const itemName = itemNameInput.value.trim();
            const quantity = parseInt(itemQuantityInput.value);
            const type = transactionTypeSelect.value;
            const date = new Date().toISOString();
            const address = type === 'shipping' ? destinationAddressInput.value : '';

            // Create new transaction
            const transaction = {
                id: Date.now(),
                itemName,
                quantity,
                type,
                date,
                address
            };
            
            // Add to transactions array
            transactions.push(transaction);
            
            // Save to localStorage
            localStorage.setItem('transactions', JSON.stringify(transactions));
            
            // Update all UI views
            renderRecentTransactions();
            generateStockReport(); // Update stock report immediately
            renderAllTransactions(); // Update full transaction list immediately
            
            // Reset form
            form.reset();
            itemNameInput.focus();
            
            // Show notification
            showNotification('Transaksi berhasil ditambahkan');
        });
        
        // Function to render recent transactions (last 5)
        function renderRecentTransactions() {
            transactionList.innerHTML = '';
            emptyTransactionMessage.classList.add('hidden');
            
            const recentTransactions = [...transactions].reverse().slice(0, 5);
            
            if (recentTransactions.length === 0) {
                emptyTransactionMessage.classList.remove('hidden');
                return;
            }
            
            recentTransactions.forEach(transaction => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${transaction.itemName}</td>
                    <td>${transaction.quantity}</td>
                    <td><span class="status-badge ${transaction.type}">${transactionLabels[transaction.type]}</span></td>
                    <td>${formatDate(transaction.date)}</td>
                    <td>${transaction.address || '-'}</td>
                `;
                transactionList.appendChild(row);
            });
        }
        
        // Function to generate stock report
        function generateStockReport(cutoffDate = null) {
            stockList.innerHTML = '';
            emptyStockMessage.classList.add('hidden');
            
            // Filter transactions by date if cutoffDate is provided
            let filteredTransactions = transactions;
            if (cutoffDate) {
                filteredTransactions = transactions.filter(t => new Date(t.date) <= new Date(cutoffDate));
            }
            
            if (filteredTransactions.length === 0) {
                emptyStockMessage.classList.remove('hidden');
                return;
            }
            
            // Calculate stock levels
            const stockLevels = {};
            
            filteredTransactions.forEach(transaction => {
                const { itemName, quantity, type } = transaction;
                
                if (!stockLevels[itemName]) {
                    stockLevels[itemName] = 0;
                }
                
                // Update stock based on transaction type
                switch (type) {
                    case 'in':
                    case 'return':
                        stockLevels[itemName] += quantity;
                        break;
                    case 'out':
                    case 'shipping':
                        stockLevels[itemName] -= quantity;
                        break;
                }
            });
            
            // Render stock report
            Object.entries(stockLevels).forEach(([item, stock]) => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${item}</td>
                    <td>${stock}</td>
                `;
                stockList.appendChild(row);
            });
        }
        
        // Function to render all transactions
        function renderAllTransactions() {
            fullTransactionList.innerHTML = '';
            emptyFullTransactionMessage.classList.add('hidden');
            
            if (transactions.length === 0) {
                emptyFullTransactionMessage.classList.remove('hidden');
                return;
            }
            
            [...transactions].reverse().forEach(transaction => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${transaction.itemName}</td>
                    <td>${transaction.quantity}</td>
                    <td><span class="status-badge ${transaction.type}">${transactionLabels[transaction.type]}</span></td>
                    <td>${formatDate(transaction.date)}</td>
                    <td>${transaction.address || '-'}</td>
                `;
                fullTransactionList.appendChild(row);
            });
        }
        
        // Date formatter
        function formatDate(isoString) {
            const date = new Date(isoString);
            return date.toLocaleString('id-ID', {
                day: '2-digit',
                month: '2-digit',
                year: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
        }
        
        // Event listener for stock report button
        reportButton.addEventListener('click', () => {
            const reportDate = reportDateInput.value;
            generateStockReport(reportDate ? new Date(reportDate) : null);
        });
        
        // Event listener for transaction report button
        transactionReportButton.addEventListener('click', () => {
            renderAllTransactions();
        });
        
        // Show notification
        function showNotification(message) {
            alert(message); // Replace with a better UI notification in real use
        }
        
        // Initial renders
        renderRecentTransactions();
        generateStockReport(); // Show initial stock report without date filter
        renderAllTransactions(); // Show all transactions immediately
    </script>
</body>
</html>
