<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OPR Generator Mobile</title>
    <style>
        /* Mobile-First Form Styling */
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f4f7f6; margin: 0; padding: 20px; color: #333; }
        .container { max-width: 600px; margin: auto; background: white; padding: 25px; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); }
        h2 { text-align: center; color: #2c3e50; margin-bottom: 5px; }
        .subtitle { text-align: center; color: #7f8c8d; font-size: 14px; margin-bottom: 25px; }
        
        label { font-weight: 600; display: block; margin-top: 15px; font-size: 14px; }
        input[type="text"], input[type="date"], textarea, input[type="file"] { 
            width: 100%; padding: 12px; margin-top: 6px; border: 1px solid #ced4da; 
            border-radius: 6px; box-sizing: border-box; font-family: inherit;
        }
        textarea { resize: vertical; }
        
        button { 
            background: #007bff; color: white; border: none; padding: 15px; 
            width: 100%; margin-top: 25px; border-radius: 6px; cursor: pointer; 
            font-size: 16px; font-weight: bold; transition: background 0.3s;
        }
        button:hover { background: #0056b3; }
        
        /* Print/PDF Output Layout */
        #report-section { display: none; background: white; padding: 40px; max-width: 800px; margin: auto; }
        .opr-header { text-align: center; border-bottom: 3px solid #000; padding-bottom: 10px; margin-bottom: 20px; font-size: 24px; font-weight: bold; }
        
        .opr-table { width: 100%; border-collapse: collapse; margin-bottom: 25px; font-size: 14px; }
        .opr-table th, .opr-table td { border: 1px solid #000; padding: 12px; text-align: left; vertical-align: top; }
        .opr-table th { background-color: #f8f9fa; width: 30%; text-transform: uppercase; }
        
        .img-title { text-align: center; font-weight: bold; text-transform: uppercase; margin-bottom: 15px; }
        .images-container { display: flex; gap: 20px; justify-content: center; }
        .img-preview { max-width: 45%; height: auto; max-height: 250px; border: 1px solid #ddd; padding: 5px; }

        /* Hide the form and format the page when Print to PDF is triggered */
        @media print {
            body { background: white; padding: 0; margin: 0; }
            .container { display: none !important; }
            #report-section { display: block !important; padding: 0; }
            @page { margin: 1.5cm; }
        }
    </style>
</head>
<body>

<!-- INPUT FORM SECTION -->
<div class="container" id="form-section">
    <h2>Sistem OPR Generator</h2>
    <div class="subtitle">Jana One Page Report (Mudah Alih)</div>
    
    <form id="oprForm">
        <label>Nama Program / Aktiviti:</label>
        <input type="text" id="namaProgram" placeholder="Cth: Perkhemahan Unit Beruniform" required>
        
        <label>Tarikh:</label>
        <input type="date" id="tarikh" required>
        
        <label>Tempat:</label>
        <input type="text" id="tempat" placeholder="Cth: Padang Sekolah" required>
        
        <label>Objektif Program:</label>
        <textarea id="objektif" rows="3" placeholder="Senaraikan objektif..." required></textarea>
        
        <label>Aktiviti yang Dijalankan:</label>
        <textarea id="aktiviti" rows="3" placeholder="Ringkasan aktiviti..." required></textarea>
        
        <label>Kekuatan / Kelebihan:</label>
        <textarea id="kekuatan" rows="2" placeholder="Kekuatan program..." required></textarea>
        
        <label>Kelemahan / Kekangan:</label>
        <textarea id="kelemahan" rows="2" placeholder="Kekangan semasa program..." required></textarea>
        
        <label>Cadangan Penambahbaikan:</label>
        <textarea id="penambahbaikan" rows="2" placeholder="Tindakan susulan..." required></textarea>

        <label>Gambar 1 (Pilihan):</label>
        <input type="file" id="gambar1" accept="image/*">

        <label>Gambar 2 (Pilihan):</label>
        <input type="file" id="gambar2" accept="image/*">
        
        <button type="button" onclick="generateOPR()">Jana OPR & Simpan PDF</button>
    </form>
</div>

<!-- REPORT OUTPUT SECTION (Hidden until generated) -->
<div id="report-section">
    <div class="opr-header">ONE PAGE REPORT (OPR)</div>
    <table class="opr-table">
        <tr>
            <th>Nama Program</th>
            <td id="out-nama"></td>
        </tr>
        <tr>
            <th>Tarikh</th>
            <td id="out-tarikh"></td>
        </tr>
        <tr>
            <th>Tempat</th>
            <td id="out-tempat"></td>
        </tr>
        <tr>
            <th>Objektif</th>
            <td id="out-objektif"></td>
        </tr>
        <tr>
            <th>Aktiviti</th>
            <td id="out-aktiviti"></td>
        </tr>
        <tr>
            <th>Kekuatan</th>
            <td id="out-kekuatan"></td>
        </tr>
        <tr>
            <th>Kelemahan</th>
            <td id="out-kelemahan"></td>
        </tr>
        <tr>
            <th>Penambahbaikan</th>
            <td id="out-penambahbaikan"></td>
        </tr>
    </table>
    
    <div>
        <div class="img-title">Gambar Sekitar Program</div>
        <div class="images-container">
            <img id="out-img1" class="img-preview" style="display:none;">
            <img id="out-img2" class="img-preview" style="display:none;">
        </div>
    </div>
</div>

<script>
    function generateOPR() {
        // Transfer data from inputs to the report table
        document.getElementById('out-nama').innerText = document.getElementById('namaProgram').value;
        document.getElementById('out-tarikh').innerText = document.getElementById('tarikh').value;
        document.getElementById('out-tempat').innerText = document.getElementById('tempat').value;
        document.getElementById('out-objektif').innerText = document.getElementById('objektif').value;
        document.getElementById('out-aktiviti').innerText = document.getElementById('aktiviti').value;
        document.getElementById('out-kekuatan').innerText = document.getElementById('kekuatan').value;
        document.getElementById('out-kelemahan').innerText = document.getElementById('kelemahan').value;
        document.getElementById('out-penambahbaikan').innerText = document.getElementById('penambahbaikan').value;

        // Process image uploads for the preview
        let file1 = document.getElementById('gambar1').files[0];
        let file2 = document.getElementById('gambar2').files[0];

        if (file1) {
            document.getElementById('out-img1').src = URL.createObjectURL(file1);
            document.getElementById('out-img1').style.display = "block";
        }
        if (file2) {
            document.getElementById('out-img2').src = URL.createObjectURL(file2);
            document.getElementById('out-img2').style.display = "block";
        }

        // Hide input form and show the clean report layout
        document.getElementById('form-section').style.display = 'none';
        document.getElementById('report-section').style.display = 'block';
        
        // Trigger browser print dialog (User can select "Save as PDF")
        window.print();
        
        // Automatically switch back to the form after the print dialog closes
        setTimeout(() => {
            document.getElementById('form-section').style.display = 'block';
            document.getElementById('report-section').style.display = 'none';
        }, 1000);
    }
</script>

</body>
</html>
