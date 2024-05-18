<div dir="rtl">

# سند فنی پیشنهاددهی بی‌درنگ دیوار (RTB)

## مقدمه

این سند جهت شفاف‌سازی نحوهٔ اتصال مشتری‌های سمت تقاضا (DSP) به سرویس مزایدهٔ (Exchange) دیوار تدوین شده است. کلیات سند برداشته شده از نسخهٔ ۳ استاندارد OpenRTB و مطابق استانداردهای موجود در صنعت تبلیغات است.
پروتکل ارتباطی پشتیبانی شده برای Request و Response اصلی پیام‌های protobuf روی gRPC است. تعاریف پیام‌های protobuf به این سند ضمیمه خواهد شد. لازم به ذکر است صدا شدن کال‌بک‌ها به صورت HTTP عادی صورت می‌گیرد.

## درخواست ارسالی از سمت دیوار (Request)

### درخواست (Request)
<div dir="ltr">

 ``` protobuf
message Request {
  // Unique ID of the bid request; provided by the exchange.
  // Required by the OpenRTB specification.
  string id = 1;

  // Array of Item objects (at least one) that constitute the set of goods being offered for sale.
  // Required by the OpenRTB specification.
  repeated Item item = 10;

  // Layer-4 domain object structure that provides context for the items being offered .
  // Recommended by the OpenRTB specification.
  Context context = 12;
}
```
<div dir="rtl">

### مورد مزایده (Item)
<div dir="ltr">

``` protobuf
message Item {
  // A unique identifier for this item within the context of the offer (typically starts with "1" and increments).
  // Required by the OpenRTB specification.
  string id = 1;


  // Minimum bid price for this item expressed in CPM.
  double flr = 5;

  // Layer-4 domain object structure that specifies the item being offered.
  // Recommended by the OpenRTB specification.
  adcom.v1.placement.Placement spec = 13;
}
```
<div dir="rtl">

### ‫مضمون تبلیغات (Context)
<div dir="ltr">

``` protobuf
message Context {
  adcom.v1.context.DistributionChannel app = 1;
  adcom.v1.context.DistributionChannel site = 2;
  adcom.v1.context.User user = 3;
  adcom.v1.context.Device device = 4;
}

```
<div dir="rtl">

### نمونه درخواست ارسالی از سمت دیوار
<div dir="ltr">

``` json
{
  "id": "random-request-id",
  "item": [
    {
      "id": "random-request-id",
      "flr": 300,
      "spec": {}
    }
  ],
  "context": {
    "device": {
      "ifa": "id-for-advertising"
    },
    "app": {
      "app": {
        "bundle": "ir.divar"
      },
      "content": {
        "data": [
          {
            "id": "divar",
            "name": "divar",
            "segment": [
              {
                "id": "search-query",
                "name": "search query",
                "value": "مبلمان"
              },
              {
                "id": "filter-city",
                "name": "location filter",
                "value": "1"
              },
              {
                "id": "filter-category",
                "name": "category filter",
                "value": "furniture"
              },
              {
                "id": "brand-models",
                "name": "brand models",
                "value": "Pride 141 i"
              },
              {
                "id": "filter-neighborhood",
                "name": "neighborhood filter",
                "value": "907"
              },
              {
                "id": "filter-price-min",
                "name": "price min filter",
                "value": "100000000"
              },
              {
                "id": "filter-price-max",
                "name": "price max filter",
                "value": "150000000"
              },
              {
                "id": "filter-poduction-year-min",
                "name": "production year min filter",
                "value": "1399"
              },
              {
                "id": "filter-production-year-max",
                "name": "proudction year max filter",
                "value": "1403"
              },
              {                                                                                                                            
                "id": "filter-status",                                                                                                     
                "name": "status filter",
                "value": "new"
              }
            ]
          }
        ]
      }
    }
  }
}
```
<div dir="rtl">

- لیست فیلتر‌ها:
    - filter-city فیلتر شهر
    - search-query کوئری جست و جو شده
    - filter-category کتگوری انتخاب شده
    - brand-models برند انتخاب شده
    - filter-neighborhood محله انتخاب شده
    - filter-price-min حداقل قیمت انتخاب شده
    - filter-price-max حداکثر قیمت انتخاب شده
    - filter-poduction-year-min حداقل سال تولید
    - filter-production-year-max حداکثر سال تولید
    - filter-status وضعیت کالا (دارای مقادیر new, like-new, used, repair-needed)
- تناظر آی‌دی شهر به نام شهر در [این آدرس](https://api.divar.ir/v8/places/cities) قابل مشاهده است.
- دیتای کتگوری‌های دیوار در [این آدرس](https://api.divar.ir/v8/categories)  قابل مشاهده است.
- دیتای محله‌های دیوار در آدرس https://api.divar.ir/v8/places/cities/<CITY\_ID>/geojson (<CITY\_ID> آی‌دی شهر می‌باشد) قابل مشاهده است.
- لیست برند‌های خودرو از در [این آدرس]() قابل مشاهده است.

این دیتا به ندرت تغییر می‌کند و می‌توانید یک کپی از آن ذخیره کنید.

## پاسخ ارسالی به درخواست دیوار (Response)
### پاسخ (Response)
پاسخ سه فیلد دارد:
- id که باید برابر با request.id تنظیم شود
- bidid که توسط شما تولید می‌شود و باید یکتا باشد
- seatbid که پیشنهادهای شما برای شرکت در مزایده در آن توصیف شده‌اند

<div dir="ltr">

``` protobuf
message Response {
  // ID of the bid request to which this is a response; must match the request.id attribute.
  // Required by the OpenRTB specification.
  string id = 1;

  // Bidder generated response ID to assist with logging/tracking.
  string bidid = 2;

  // Array of Seatbid objects; 1+ required if a bid is to be made.
  repeated SeatBid seatbid = 6;
}
```
<div dir="rtl">

### پیشنهاد (SeatBid)
هر پیشنهاد-صندلی ۲ فیلد دارد
- seat نمایانگر صندلی خریدار است و لزوما با کسی که پاسخ را ارسال می‌کند یکی نیست. مثلا اگر آژانس ۱(agency_1) یک بید از طرف خودش و یک بید از طرف آژانس ۲ در پاسخ ارسال می‌کند باید در فیلد seat بید اول مقدار agency_1 و در فیلد seat بید دوم مقدار agency_2 را قرار دهد. لازم به ذکر است seat می‌تواند مقادیر محدود و مشخصی به ازای هر آژانس بگیرد. این مقادیر بر حسب قرارداد و برای هر آژانس متفاوت خواهند بود.
- bid که پیشنهاد شما برای شرکت در مزایده است. خود bid دارای چندین فیلد مهم است
  - Id که توسط پیشنهاددهنده تولید شده و برای دنبال کردن وضعیت پیشنهاد است
  - item به موردی که پیشنهاد روی آن گذاشته شده اشاره می‌کند (در مورد ما که فقط یک جایگاه بنری وجود دارد آیتم همیشه برابر request.id است)
  - price قیمت پیشنهادی برای item است (به ریال)
  - purl آدرسی است که در صورت برنده شدن این پیشنهاد در مزایده صدا زده خواهد شد
  - burl آدرسی است که در صورت تحویل ایمپرشن صدا زده خواهد شد
  - lurl آدرسی است که در صورت باخت یشنهاد در مزایده صدا زده خواهد شد
  - mid آی‌دی آگهی در سامانه مدیریت آگهی‌ها.
  - macro ماکرو‌های مشتری که در لینک‌ها (شامل ترکر‌ها و کالبک‌ها) جایگزین می‌شوند.
  
<div dir="ltr">

``` protobuf
message SeatBid {
  // ID of the buyer seat on whose behalf this bid is made.
  // Recommended by the OpenRTB specification.
  string seat = 1;

  // Array of 1+ Bid objects each related to an item. Multiple bids can relate to the same item.
  // Required by the OpenRTB specification.
  repeated Bid bid = 3;
}

message Bid {
  // Bidder generated bid ID to assist with logging/tracking.
  // Recommended by the OpenRTB specification.
  string id = 1;

  // ID of the item object in the related bid request; specifically item.id.
  // Required by the OpenRTB specification.
  string item = 2;

  // Bid price expressed as CPM although the actual transaction is for a unit item only.
  // Required by the OpenRTB specification.
  double price = 3;

  // Pending notice URL called by the exchange when a bid has been declared the winner within
  // the scope of an OpenRTB compliant supply chain (i.e., there may still be non-compliant
  // decisioning such as header bidding). Substitution macros may be included.
  string purl = 7;

  // Billing notice URL called by the exchange when a winning bid becomes billable based on
  // exchange-specific business policy (e.g., markup rendered). Substitution macros may be
  // included.
  string burl = 8;

  // ID to enable media to be specified by reference if previously uploaded to
  // the exchange rather than including it by value in the domain objects.
  string mid = 11;

  // Array of Macro objects that enable bid specific values to be substituted
  // into markup; especially useful for previously uploaded media referenced via
  // the mid attribute. Refer to Object: Macro.
  repeated Macro macro = 12;
}

message Macro {
  // Name of a buyer specific macro.
  string key = 1;

  // Value to substitute for each instance of the macro found in markup.
  string value = 2;
}

```
<div dir="rtl">

### کالبک باخت (Loss Notice)
دلیل باخت در مزایده در ماکروی {OPENRTB\_LOSS}\$ در lurl جایگزین می‌شود. برای مثال می‌توانید در پاسخ خود lurl را برابر https://loss.reason/?lr=${OPENRTB_LOSS} قرار دهید و انتظار صدا زده شدن آدرس به این صورت را داشته باشید: https://loss.reason/?lr=2.
#### دلایل باخت
دلایل باخت مبتنی بر [دلایل باخت استاندارد OpenRTB](https://github.com/InteractiveAdvertisingBureau/openrtb/blob/main/OpenRTB%20v3.0%20FINAL.md#list--loss-reason-codes-) تنظیم شده‌اند. دلایل باخت فعلی سیستم به شرح زیر هستند:
- 3: Invalid Bid Response
- 100: Bid was Below Auction Floor
- 102: Lost to Higher Bid
- 200: Creative Filtered - General; Reason Unknown
- 201: Creative Pending
- 202: Creative Disapproved


### نمونه پاسخ ارسالی به درخواست دیوار
<div dir="ltr">

``` json
{
  "seatbid": [
    {
      "bid": [
        {
          "id": "random-bid-id",
          "item": "random-request-id",
          "price": 5000,
          "mid": "your-media_id",
          "macro": [
            {
              "key": "KEY_1",
              "value": "value1"
            },
            {
              "key": "KEY_2",
              "value": "value2"
            }
          ]
        }
      ],
      "seat": "your-seat"
    }
  ],
  "id": "random-request-id",
  "bidid": "random-bidder-response-id"
}
```
<div dir="rtl">

