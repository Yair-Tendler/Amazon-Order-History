# Amazon-Order-History
Create a CSV  of your Amazon order History For buyers not sellers with OrderID/QTY/images/tracking#
Amazon recently has deleted the option download your order history to a CSV file https://www.amazon.com/gp/b2b/reports
it now only let you view it in https://www.amazon.com/gp/your-account/order-history


How to use 
-----------
Step 1.

Load amazon.com in your chrome browser go to console (F12) and paste the code below, it will output the last 100 orders with tracking number/ASIN/OrderID/Amount/QTY/Description to the console screen.

Step2.
Paste the console output to a file. and import to excel or you DB using the  delimiter '|||' 

P.S.
you can use console.save (google it) or if you feel like making your browser a major security risk enable Access-Control-Allow-Origin and transmit it directly to your API.


-------- code to paste in console when your browser is in amazon.com ------
var arrURL = ['https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=0', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_2?ie=UTF8&orderFilter=months-6&search=&startIndex=10', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_3?ie=UTF8&orderFilter=months-6&search=&startIndex=20', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_4?ie=UTF8&orderFilter=months-6&search=&startIndex=30', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_5?ie=UTF8&orderFilter=months-6&search=&startIndex=40', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_6?ie=UTF8&orderFilter=months-6&search=&startIndex=50', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_7?ie=UTF8&orderFilter=months-6&search=&startIndex=60', 'https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=70'];
var arrURL2 = ['https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=80']
var counter1=0
//var arrURL = ['https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=0'];
//var arrURL2 = ['https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=80'];


var outputdata2file='';
//var getDoc = getSourceAsDOM('https://www.amazon.com/gp/your-account/order-history/ref=ppx_yo_dt_b_pagination_1_8?ie=UTF8&orderFilter=months-6&search=&startIndex=0');

for (var i = 0; i < arrURL.length; i++) {
    checkAmaData(arrURL[i]);

}

for (var j = 0; i < arrURL2.length; j++) {
    checkAmaData(arrURL[j]);

}


function checkAmaData(myurl) {

    var getDoc = getSourceAsDOM(myurl);

    var getBody = getDoc.body;
    //console.log(getBody.getElementsByClassName('order'));
    var getOrders = getBody.getElementsByClassName('order');

    Array.from(getOrders).forEach((myOrder, index) => {

        var orderPlacedDate = myOrder.getElementsByClassName('a-color-secondary value')[0].innerText;
        var orderNumber =  myOrder.getElementsByTagName('bdi')[0].innerText;
        var getTrackingButton = myOrder.querySelectorAll('.track-package-button a');
        var myTrackingID = "No Tracking ID Yet";
        if (getTrackingButton === undefined || getTrackingButton.length == 0)  {
                getTrackingButton = 'No Tracking Yet'
        }
        else {
                getTrackingButton = myOrder.querySelectorAll('.track-package-button a')[0].href;
                var getDOMTracking = getSourceAsDOM(getTrackingButton);
                var getBodyDOMTracking = getDOMTracking.body;
                var getTrackingDetails = getBodyDOMTracking.getElementsByClassName('a-spacing-small carrierRelatedInfo-trackingId-text');
                if (getTrackingDetails !== undefined && getTrackingDetails.length != 0)  {
                    myTrackingID = getBodyDOMTracking.getElementsByClassName('a-spacing-small carrierRelatedInfo-trackingId-text')[0].innerText;
                }
                
                
        }

        
    
        var getAllProducts = myOrder.getElementsByClassName('a-fixed-left-grid-inner');



    Array.from(getAllProducts).forEach((order, myIndex) => {


                var getSignleItem = order;
//                  console.log(order);
                
                var getDescriptionPrice = getSignleItem.getElementsByClassName('a-fixed-left-grid-col a-col-right')[0];
               
                
                var itemDescription = getDescriptionPrice.getElementsByClassName('a-link-normal')[0].innerText;
                console.log(itemDescription.trim());


                var itemPrice = getDescriptionPrice.getElementsByClassName('a-size-small a-color-price')[0].innerText;
//                 console.log(itemPrice.trim());

                

            

                var getImgQty = getSignleItem.getElementsByClassName('item-view-left-col-inner')[0];
               
                    
//                 console.log(getImgQty);


                var getImg = getImgQty.querySelectorAll('.item-view-left-col-inner img')[0].getAttribute('data-a-hires');
//                 console.log("_______________________")
//                 console.log(getImg);

                var getAsianLink = getImgQty.querySelectorAll('.a-link-normal')[0].getAttribute('href');
                var mySubStringAsian = getAsianLink.substring(
                    getAsianLink.lastIndexOf("/product/") + 9, 
                    getAsianLink.lastIndexOf("/ref")
                );
//                 console.log(mySubStringAsian);


                var getQty = getImgQty.getElementsByClassName('item-view-qty');
//                 console.log(getImgQty);
                
                
                if (getQty === undefined || getQty.length == 0)  {
                    getQty = 1;
                }
                else {
//                     console.log('i am here - i have qty listed');
   
                    getQty = getImgQty.getElementsByClassName('item-view-qty')[0].innerText.trim();
//                     console.log(getQty);
           }


          /* added ASIAN NUMBER & myTrackingID */

          outputdata2file=outputdata2file + orderPlacedDate.trim() + '||||' + orderNumber.trim() + '||||' + itemDescription.trim() + '||||' +  getImg.trim() + '||||' +  getQty + '||||' + itemPrice.trim()  + '||||' + getTrackingButton.trim() +'||||' + mySubStringAsian + '||||'+ myTrackingID + '\n';
          counter1=counter1+1;
         console.clear();
          console.log('Completed ' + counter1 + '%');
        });



    });

}



function getSourceAsDOM(url)
{
    xmlhttp=new XMLHttpRequest();
    xmlhttp.open("GET",url,false);
    xmlhttp.send();
    parser=new DOMParser();
    return parser.parseFromString(xmlhttp.responseText,"text/html");      
}


console.clear();
console.log(outputdata2file)

------- END --------
