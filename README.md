# pdfimg.github.io
<!DOCTYPE html>
<html>
<head>
    <title>Imagens para PDF</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.4/jspdf.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>




    </head>
<body>
    <h1>Converter Imagens em PDF</h1>
    
    <input type="file" id="imgInput" multiple>
    <button type="submit" id="generatePdf">Gerar PDF</button>
    
    <script>
        document.getElementById('generatePdf').addEventListener('click', async function() {
            const imgInput = document.getElementById('imgInput');
            const images = imgInput.files;
            
            if (images.length === 0) {
                alert('Nenhuma imagem selecionada.');
                return;
            }
            
            const pdfPromises = [];
            
            for (let i = 0; i < images.length; i++) {
                const image = images[i];
                const imgData = await convertImageToDataUrl(image);
                const imgHtml = `<img src="${imgData}" width="500">`;
                
                const pdfPromise = html2pdf().from(imgHtml).outputPdf();
                pdfPromises.push(pdfPromise);
            }
            
            Promise.all(pdfPromises).then(pdfBlobs => {
                pdfBlobs.forEach((pdfBlob, index) => {
                    const pdfFileName = `imagem_${index}.pdf`;
                    saveAs(pdfBlob, pdfFileName);
                });
            });
        });
        
        function convertImageToDataUrl(image) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = function(event) {
                    resolve(event.target.result);
                };
                reader.readAsDataURL(image);
            });
        }
    </script>
</body>
</html>
