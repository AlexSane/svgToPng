
https://mavrin.github.io/svgToPng

# Convet svg to png

������ ���� ������������� ��������� svg � png ���������� ��������. � ��������� ������� �� ����� ���������� api, ������� �������� �� ��� ������� ��� ��������� �����. ��� �� ������, ���� ��� ���� ������� �������� ���������?  ![](http://habrastorage.org/storage2/d36/922/412/d36922412b65ad20413ac591db392e09.png)
������ ����, ������� ��� ������ � ������, ������� ��� ����� canvas, ������� ����� ����� `toDataURL('image/png');`
� ��� � ������� ����������� ������ [jsfiddle](http://jsfiddle.net/a9ude9p0/6/), [github](http://mavrin.github.io/svgToPng/fromImage.html):
```javascript
var html = document.querySelector("svg").parentNode.innerHTML;
    var imgsrc = 'data:image/svg+xml;base64,' + btoa(html);
    var canvas = document.querySelector("canvas"),
            context = canvas.getContext("2d");
    canvas.setAttribute('width', 526);
    canvas.setAttribute('height', 233);

    var image = new Image;
    image.src = imgsrc;
    image.onload = function () {
        context.drawImage(image, 0, 0);
        var canvasdata = canvas.toDataURL("image/png");
        var a = document.createElement("a");
        a.textContent = "save";
        a.download = "export_" + Date.now() + ".png";
        a.href = canvasdata;
        document.body.appendChild(a);
        canvas.parentNode.removeChild(canvas);
    };
```
���� �������� ������, � �������������� svg � dataUri, �������� ��� ����� image, ������� �������� �� canvas � ��������� � png. �������� ���� ����������, � ����� �����������. ���� ������ �������� � Firefox � chrome, �� ������ ~~�� ����� ���� ������� ��������~~ IE, � ������� ������������� ������:
![secureError](http://habrastorage.org/files/1c6/74c/de8/1c674cde8f51425a82a653e55e86bc9e.png) 
���� � ��� ��� IE �������, ��� �������� ��������� � ������� �����.  � ��������� ���������� origin ��� dataUri �� ���������. ���������� �������� ������ https://html.spec.whatwg.org/multipage/scripting.html#security-with-canvas-elements. ����� ���� ������� ������������ svg ����� ������ � ����� ��� �� ���������, �� �������� ����� �������� �������.
� ��� � �������� ��� ������������� ���������� [canvg](https://github.com/gabelerner/canvg). � ������� ���� ���������� � ����� svg �� canvas, � ����� �������� � � ������ ������ ���� `toDataURL("image/png")`. ��������� ����� �� ������������ ��� [github](http://mavrin.github.io/svgToPng/useCanvg.html):
```javascript
 var svg = document.querySelector('svg');
        var canvas = document.createElement('canvas');
        canvas.height = svg.getAttribute('height');
        canvas.width = svg.getAttribute('width');
        canvg(canvas, svg.parentNode.innerHTML.trim());
        var dataURL = canvas.toDataURL('image/png');
        var data = atob(dataURL.substring('data:image/png;base64,'.length)),
                asArray = new Uint8Array(data.length);

        for (var i = 0, len = data.length; i < len; ++i) {
            asArray[i] = data.charCodeAt(i);
        }

        var blob = new Blob([asArray.buffer], {type: 'image/png'});
        saveAs(blob, 'export_' + Date.now() + '.png');
```
��� ����� �������, ��� � ��� ����������� ���������� [FileSaver](https://github.com/ChenWenBrian/FileSaver.js) ��� ������ ����������� ���� ����������.
��� � ���, �� �������� ��������� ����������.
����� �������� ���� �����, � ������� �������� ���������� svg � png, ����� ����� ������ ��� �������� [tauCharts](http://www.taucharts.com/). ��� ��� ����� � svg �������� �� �������� �����, ��� �� �������� ����������� ������� � �������� svg, � �������� inline style � svg. B �������� ��� ����� 
[���������](http://mavrin.github.io/svgToPng/tauChartsExample.html)

������� ������ �������� �������� ��� ��� � �������� ��� �����.
