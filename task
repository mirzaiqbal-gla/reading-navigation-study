/*
============================================================================
REVISED JAVASCRIPT CODE - DRAWING TASK 1
============================================================================
1. Copy this entire code
2. Paste into Drawing Task 1's JavaScript editor in Qualtrics
3. For Drawing Tasks 2, 3, 4: Copy this code and change the CONFIGURATION section
============================================================================
*/

Qualtrics.SurveyEngine.addOnload(function () {
    var qthis = this;
    var container = qthis.getQuestionContainer();
    
    // ========================================================================
    // CONFIGURATION - CHANGE FOR EACH DRAWING TASK
    // ========================================================================
    var TASK_NUMBER = 1;  // Drawing Task 1 → 1, Task 2 → 2, Task 3 → 3, Task 4 → 4
    
    // Metadata - tells us which manuscript this is
    var ITEM = 'Item1';           // Item1, Item2, Item3, or Item4
    var LANGUAGE = 'English';      // English or Arabic
    var CONDITION = 'Messy';       // Messy or Clear
    
    // Standardized image dimensions (if all images are same size)
    var STANDARD_WIDTH = 2063;
    var STANDARD_HEIGHT = 1513;
    // ========================================================================
    
    // Hide the text input box (but it will still save data)
    var textArea = container.querySelector('textarea');
    if (textArea) {
        textArea.style.display = 'none';
        textArea.parentElement.style.display = 'none';
    }

    var canvas = container.querySelector('#annotCanvas');
    var img = container.querySelector('#manuscriptImage');

    if (!canvas || !img) {
        console.log('Canvas or image not found');
        return;
    }

    var ctx = canvas.getContext('2d');

    var imgState = {
        scale: 1,
        offsetX: 0,
        offsetY: 0,
        imgWidth: 0,
        imgHeight: 0
    };

    var shapes = [];

    function getCanvasPos(evt) {
        var rect = canvas.getBoundingClientRect();
        var clientX = evt.touches ? evt.touches[0].clientX : evt.clientX;
        var clientY = evt.touches ? evt.touches[0].clientY : evt.clientY;

        // Scale mouse position to account for CSS scaling
        var scaleX = canvas.width / rect.width;
        var scaleY = canvas.height / rect.height;

        return {
            x: (clientX - rect.left) * scaleX,
            y: (clientY - rect.top) * scaleY
        };
    }

    function imageToCanvasCoords(ix, iy) {
        return {
            x: ix * imgState.scale + imgState.offsetX,
            y: iy * imgState.scale + imgState.offsetY
        };
    }

    function saveData() {
        try {
            // REVISED: Include metadata in JSON
            var jsonString = JSON.stringify({
                // Metadata - identifies which manuscript
                item: ITEM,
                language: LANGUAGE,
                condition: CONDITION,
                
                // Image dimensions
                imgWidth: STANDARD_WIDTH,
                imgHeight: STANDARD_HEIGHT,
                
                // Canvas dimensions (for reference/debugging)
                canvasWidth: canvas.width,
                canvasHeight: canvas.height,
                
                // Additional info
                timestamp: new Date().toISOString(),
                rectangleCount: shapes.length,
                
                // Rectangle data
                rects: shapes
            });
            
            // Save to the hidden textarea
            if (textArea) {
                textArea.value = jsonString;
            }
            
            // REVISED: Save to numbered field instead of condition-specific field
            var fieldName = 'rectangleData_' + TASK_NUMBER;
            Qualtrics.SurveyEngine.setEmbeddedData(fieldName, jsonString);
            
            console.log("✓ Data saved to " + fieldName + "! Rectangle count:", shapes.length);
            
        } catch (e) {
            console.error('❌ Error saving data:', e);
        }
    }

    function redrawAll(currentRect) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(
            img,
            imgState.offsetX,
            imgState.offsetY,
            imgState.imgWidth,
            imgState.imgHeight
        );

        ctx.lineWidth = 2;
        ctx.strokeStyle = 'red';
        ctx.fillStyle = 'rgba(255, 0, 0, 0.15)';

        shapes.forEach(function (r) {
            var p1 = imageToCanvasCoords(r.x, r.y);
            var p2 = imageToCanvasCoords(r.x + r.width, r.y + r.height);
            var w = p2.x - p1.x;
            var h = p2.y - p1.y;
            ctx.fillRect(p1.x, p1.y, w, h);
            ctx.strokeRect(p1.x, p1.y, w, h);
        });

        if (currentRect) {
            ctx.fillRect(currentRect.x, currentRect.y, currentRect.width, currentRect.height);
            ctx.strokeRect(currentRect.x, currentRect.y, currentRect.width, currentRect.height);
        }
    }

    img.onload = function () {
        var cw = canvas.width;
        var ch = canvas.height;

        var iw = img.naturalWidth;
        var ih = img.naturalHeight;

        var scale = Math.min(cw / iw, ch / ih);
        var imgW = iw * scale;
        var imgH = ih * scale;
        var offsetX = (cw - imgW) / 2;
        var offsetY = (ch - imgH) / 2;

        imgState.scale = scale;
        imgState.offsetX = offsetX;
        imgState.offsetY = offsetY;
        imgState.imgWidth = imgW;
        imgState.imgHeight = imgH;

        redrawAll();
        console.log("✓ Image loaded successfully:", iw, "x", ih);
    };

    if (img.complete) {
        img.onload();
    }

    var isDrawing = false;
    var startPos = null;

    function pointerDown(evt) {
        evt.preventDefault();
        isDrawing = true;
        startPos = getCanvasPos(evt);
    }

    function pointerMove(evt) {
        if (!isDrawing) return;
        evt.preventDefault();
        var pos = getCanvasPos(evt);
        var x = Math.min(startPos.x, pos.x);
        var y = Math.min(startPos.y, pos.y);
        var w = Math.abs(startPos.x - pos.x);
        var h = Math.abs(startPos.y - pos.y);
        redrawAll({ x: x, y: y, width: w, height: h });
    }

    function pointerUp(evt) {
        if (!isDrawing) return;
        evt.preventDefault();
        isDrawing = false;
        var pos = getCanvasPos(evt);
        var x = Math.min(startPos.x, pos.x);
        var y = Math.min(startPos.y, pos.y);
        var w = Math.abs(startPos.x - pos.x);
        var h = Math.abs(startPos.y - pos.y);

        if (w < 5 || h < 5) {
            redrawAll();
            return;
        }

        var ix1 = (x - imgState.offsetX) / imgState.scale;
        var iy1 = (y - imgState.offsetY) / imgState.scale;
        var ix2 = (x + w - imgState.offsetX) / imgState.scale;
        var iy2 = (y + h - imgState.offsetY) / imgState.scale;

        shapes.push({
            x: ix1,
            y: iy1,
            width: ix2 - ix1,
            height: iy2 - iy1
        });

        console.log("✓ Rectangle #" + shapes.length + " added:", shapes[shapes.length - 1]);
        redrawAll();
        saveData(); // Save after each rectangle
    }

    canvas.addEventListener('mousedown', pointerDown);
    canvas.addEventListener('mousemove', pointerMove);
    canvas.addEventListener('mouseup', pointerUp);

    canvas.addEventListener('touchstart', pointerDown, { passive: false });
    canvas.addEventListener('touchmove', pointerMove, { passive: false });
    canvas.addEventListener('touchend', pointerUp, { passive: false });
});

// Final save when leaving the question
Qualtrics.SurveyEngine.addOnUnload(function () {
    // ========================================================================
    // CONFIGURATION - MUST MATCH THE CONFIGURATION IN addOnload ABOVE
    // ========================================================================
    var TASK_NUMBER = 1;  // Drawing Task 1 → 1, Task 2 → 2, Task 3 → 3, Task 4 → 4
    // ========================================================================
    
    var container = this.getQuestionContainer();
    var textArea = container.querySelector('textarea');
    
    if (textArea && textArea.value) {
        var fieldName = 'rectangleData_' + TASK_NUMBER;
        Qualtrics.SurveyEngine.setEmbeddedData(fieldName, textArea.value);
        console.log("✓ Final save complete in addOnUnload to " + fieldName);
    }
});
