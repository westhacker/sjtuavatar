# sjtuavatar
html5 canvas generate avatar output image

## Introduction
This application is developed to celebrate the 120th birthday of Shanghai Jiao Tong University. For SJTUers, they can make their own sjtu-avatar through this app.
![That's all Folks!](http://images.google.fr/imgres?imgurl=http%3A%2F%2Fsmd.sjtu.edu.cn%2Ffile%2Fdownload%2Fid%2F55a06e51e0ea3cde38e9f375&imgrefurl=http%3A%2F%2Fsmd.sjtu.edu.cn%2Fnews%2Fd%2Fid%2F56&h=914&w=826&tbnid=NTzDzYggcCsP-M%3A&docid=jz7D5XlTqn6e9M&ei=oCwBV6jhN8yzae_QrPgJ&tbm=isch&iact=rc&uact=3&dur=308&page=1&start=0&ndsp=20&ved=0ahUKEwiosenZ2vLLAhXMWRoKHW8oC58QMwgwKAowCg&safe=off)

## Solution
* JQuery
* HTML5 canvas

## Template
A mobile template by HTML5UP is used in this app.

## Function
* Load images from sources

  ```javascript
    function loadImages(sources, callback) 
    {
        var images = {};
        var loadedImages = 0;
        var numImages = 0;
        // get num of sources
        for (var src in sources) {
            numImages++;
        }
        for (var src in sources) {
            images[src] = new Image();
            images[src].onload = function () {
                if (++loadedImages >= numImages) {
                    callback(images);
                }
            };
            images[src].src = sources[src];
        }
    }
  ```
* Clip a circle area in canvas2

  ```javascript
    var canvas2 = document.getElementById('myCanvas2');
    var context2 = canvas2.getContext('2d');
    var radius = 80;
    var offset = 0;
    var x = canvas2.width / 2;
    var y = canvas2.height / 2;
    context2.save();
    context2.beginPath();
    context2.arc(x, y, radius, 0, 2 * Math.PI, false);
    context2.clip();
  ```
  
* Handle the image upload event 

  ```javascript
    function handleImage(e) 
    {
        var reader = new FileReader();
        reader.onload = function (event) {
            //image process
        }
        reader.readAsDataURL(e.target.files[0]);
    }
  ```
* Paint the uploaded image into the canvas2

  ```javascript
    var img = new Image();
    img.onload = function () 
    {
        if(img.height>img.width){
            var x = 0;
            var y = -1 * (160*img.height/img.width - 160)/2;
            var width = 160;
            var height = 160*img.height/img.width;
            context2.drawImage(img, x, y, width, height);

        }else{
            var y = 0;
            var x = -1 * (160*img.width/img.height - 160)/2;
            var height = 160;
            var width = 160*img.width/img.height;
            context2.drawImage(img, x, y, width, height);
        }
    }
  ```
  
* Paint the canvas2 into the canvas, and add sjtu logo

  ```javascript
    context.clearRect(0, 0, 280, 280);
    loadImages(sources, function (images) {
    context.fillStyle = 'white';
    context.fillRect(0, 0, 280, 280);
        
    context.drawImage(canvas2, 60, 60, 160, 160);
    context.drawImage(images.blue, 0, 0, 280, 280);
    resample_hermite(canvas, 280, 280, 280, 280);
    document.getElementById('canvasImg').src = canvas.toDataURL("image/png");
    });
  ```
  
* Output from canvas to png

  ```javascript
    document.getElementById('canvasImg').src = canvas.toDataURL("image/png");
  ```
* Logo Color Selector

  ```javascript
    $('#color').change(sources, function (images) {
        sources = {
            blue: 'images/sjtubanner_' + $("#color option:selected").val() + '.png'
        };
        //to do : repaint and generate image
    });
  ```
