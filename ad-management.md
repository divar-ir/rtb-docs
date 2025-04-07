<div dir="rtl">

# سامانه‌ مدیریت بنر‌های تبلیغاتی دیوار
این سامانه برای مدیریت بنر‌های تبلیغاتی دیوار آماده شده‌است.

## فهرست Endpointها
در این قسمت فهرست endpointهایی که برای این سامانه وجود دارد تعریف شده است. تمامی endpointها مطابق اسناد [Ad Management API](https://github.com/InteractiveAdvertisingBureau/AdManagementAPI/blob/master/Ad%20Management%20API%20v1.md) و [AdCOM](https://github.com/InteractiveAdvertisingBureau/AdCOM/blob/main/AdCOM%20v1.0%20FINAL.md) می‌باشند. برای اسکیمای ad در درخواست‌ها و پاسخ‌ها می‌توانید از پروتوی [adcom](https://github.com/divar-ir/rtb-docs/tree/main/proto/adcom/v1) استفاده کنید.

برای همگام سازی سامانه مدیریت آگهی خود با سامانه مدیریت بنر‌های دیوار می‌توانید از روش ارائه شده در [اینجا](https://github.com/InteractiveAdvertisingBureau/AdManagementAPI/blob/master/Ad%20Management%20API%20v1.md#typical-synchronization-flow-) استفاده کنید. برای این کار شما باید در فواصل زمانی مشخص API دریافت اطلاعات بنر‌ها را با فیلتر audit_start صدا بزنید. مقدار audit_start باید audit.lastmod آخرین Ad از آخرین اجرای API باشد.

متغیر‌ها:
  - baseUrl: مثدار ثابت https://api.divar.ir/ad-management
  - bidderId: شناسه آگهی گزار
  - id: شناسه بنر 
 
| Endpoint                            | Method | توضیحات             |
|-------------------------------------|--------|---------------------|
|{baseUrl}/bidder/{bidderId}/ads      | GET    | دریافت اطلاعات بنر‌ها |
|{baseUrl}/bidder/{bidderId}/ads      | POST   | ساخت بنر جدید       |	
|{baseUrl}/bidder/{bidderId}/ads/{id} | Get    | دریافت اطلاعات بنر   |
|{baseUrl}/bidder/{bidderId}/ads/{id} | PUT    | به روز رسانی بنر    |

## احراز هویت
برای احراز هویت باید از کلید‌های API حساب [کنار دیوار](https://divar.ir/kenar) در هدر X-API-KEY استفاده کنید.

## مثال‌ها
### دریافت اطلاعات بنر ها

<div dir="ltr">

``` http
GET {baseUrl}/bidder/{bidderId}/ads?audit_start={auditStart} HTTP/1.1
Accept: application/json
X-API-KEY: {api_key}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 2,
    "ads": [
        {
            "id": "id1",
            "init": "1711528034170",
            "lastmod": "1721479608368",
            "display": {
                "native": {
                    "link": {
                        "url": "https://example.org/link"
                    },
                    "asset": [
                        {
                            "image": {
                                "url": "https://example.org/image0",
                                "w": 1029,
                                "h": 504
                            }
                        },
                        {
                            "id": 1,
                            "image": {
                                "url": "https://example.org/image1",
                                "w": 984,
                                "h": 552
                            }
                        },
                        {
                            "id": 2,
                            "image": {
                                "url": "https://example.org/image2",
                                "w": 2304,
                                "h": 504
                            }
                        },
                        {
                            "id": 3,
                            "image": {
                                "url": "https://example.org/image3",
                                "w": 3408,
                                "h": 504
                            }
                        }
                    ]
                }
            },
            "audit": {
                "status": 3,
                "init": "1711528034170",
                "lastmod": "1721479608368"
            }
        },
        {
            "id": "id2",
            "init": "1711528115308",
            "lastmod": "1721479617898",
            "display": {
                "native": {
                    "link": {
                        "url": "https://example.org/link"
                    },
                    "asset": [
                        {
                            "image": {
                                "url": "https://example.org/image0",
                                "w": 1029,
                                "h": 504
                            }
                        },
                        {
                            "id": 1,
                            "image": {
                                "url": "https://example.org/image1",
                                "w": 984,
                                "h": 552
                            }
                        },
                        {
                            "id": 2,
                            "image": {
                                "url": "https://example.org/image2",
                                "w": 2304,
                                "h": 504
                            }
                        },
                        {
                            "id": 3,
                            "image": {
                                "url": "https://example.org/image3",
                                "w": 3408,
                                "h": 504
                            }
                        }
                    ]
                }
            },
            "audit": {
                "status": 3,
                "init": "1711528115308",
                "lastmod": "1721479617898"
            }
        }
    ]
}
```

<div dir="rtl"> 

### ساخت بنر جدید

#### بنر نیتیو 
 
<div dir="ltr">

``` http
POST {baseUrl}/bidder/{bidderId}/ads HTTP/1.1
Accept: application/json
X-API-KEY: {api_key}

{
    "display": {
        "native": {
            "link": {
                "url": "https://example.org/url",
                "trkr": [
                    "https://example.org/tracker"
                ]
            },
            "asset": [
                {
                    "image": {
                        "url": "https://example.org/image0",
                        "w": 1029,
                        "h": 504
                    }
                },
                {
                    "id": 1,
                    "image": {
                        "url": "https://example.org/image1",
                        "w": 984,
                        "h": 552
                    }
                },
                {
                    "id": 2,
                    "image": {
                        "url": "https://example.org/image2",
                        "w": 2304,
                        "h": 504
                    }
                },
                {
                    "id": 3,
                    "image": {
                        "url": "https://example.org/image3",
                        "w": 3408,
                        "h": 504
                    }
                }
            ]
        },
        "event": [
            {
                "type": 1,
                "url": "https://example.org/loaded"
            },
            {
                "type": 2,
                "url": "https://example.org/impression"
            },
            {
                "type": 2,
                "url": "https://example.org/impression/2?${AUCTION_PRICE}"
            },
            {
                "type": 4,
                "url": "https://example.org/mrc100"
            }
        ]
    }
}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 1,
    "ads": [
        {
            "id": "id",
            "init": "1724084590878",
            "lastmod": "1724507106939",
            "display": {
                "native": {
                    "link": {
                        "url": "https://example.org/url",
                        "trkr": [
                            "https://example.org/tracker"
                        ]
                    },
                    "asset": [
                        {
                            "image": {
                                "url": "https://example.org/image0",
                                "w": 1029,
                                "h": 504
                            }
                        },
                        {
                            "id": 1,
                            "image": {
                                "url": "https://example.org/image1",
                                "w": 984,
                                "h": 552
                            }
                        },
                        {
                            "id": 2,
                            "image": {
                                "url": "https://example.org/image2",
                                "w": 2304,
                                "h": 504
                            }
                        },
                        {
                            "id": 3,
                            "image": {
                                "url": "https://example.org/image3",
                                "w": 3408,
                                "h": 504
                            }
                        }
                    ]
                },
                "event": [
                    {
                        "type": 1,
                        "url": "https://example.org/loaded"
                    },
                    {
                        "type": 2,
                        "url": "https://example.org/impression"
                    },
                    {
                        "type": 4,
                        "url": "https://example.org/mrc100"
                    }
                ]
            },
            "audit": {
                "status": 4,
                "feedback": [
                    "لینک معتبر نیست"
                ],
                "init": "1724084590878",
                "lastmod": "1724507106939"
            }
        }
    ]
}
```

<div dir="rtl">

#### بنر بک‌گراندی

توجه کنید که ابعاد بنر ارسالی باید مطابق موارد تعیین شده باشد.

<div dir="ltr">

``` http
POST {baseUrl}/bidder/{bidderId}/ads HTTP/1.1
Accept: application/json
X-API-KEY: {api_key}

{
    "display": {
        "w": 1075,
        "h": 1584,
        "banner": {
            "img": "https://example.org/banner-image",
            "link": {
                "url": "https://example.org/url",
                "trkr": [
                    "https://example.org/tracker"
                ]
            }
        },
        "event": [
            {
                "type": 1,
                "url": "https://example.org/loaded"
            },
            {
                "type": 2,
                "url": "https://example.org/impression"
            },
            {
                "type": 2,
                "url": "https://example.org/impression/2?${AUCTION_PRICE}"
            },
            {
                "type": 4,
                "url": "https://example.org/mrc100"
            }
        ]
    }
}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 1,
    "ads": [
        {
            "id": "id",
            "init": "1724084590878",
            "lastmod": "1724507106939",
            "display": {
                 "w": 1075,
                 "h": 1584,
                "banner": {
                    "img": "https://example.org/banner-image",
                    "link": {
                        "url": "https://example.org/url",
                        "trkr": [
                            "https://example.org/tracker"
                        ],
                        "ext": {
                            "@type": "type.googleapis.com/admanagement.LinkAssetExtension"
                        }
                    },
                    "ext": {
                        "@type": "type.googleapis.com/admanagement.BannerExtension",
                        "img_public": "https://a-banners.divarcdn.com/public-image",
                        "img_public_with_badge": "https://a-banners.divarcdn.com/public-image-with-badge",
                        "type": "BANNER_TYPE_IMAGE"
                    }
                },
                "event": [
                    {
                        "type": 1,
                        "url": "https://example.org/loaded"
                    },
                    {
                        "type": 2,
                        "url": "https://example.org/impression"
                    },
                    {
                        "type": 2,
                        "url": "http://example.org/impression/2?${AUCTION_PRICE}"
                    },
                    {
                        "type": 4,
                        "url": "https://example.org/mrc100"
                    }
                ]
            },
            "audit": {
                "status": 3,
                "init": "1724084590878",
                "lastmod": "1724507106939"
            }
        }
    ]
}
```

<div dir="rtl"> 

### دریافت اطلاعات بنر

<div dir="ltr">

``` http
GET {baseUrl}/bidder/{bidderId}/ads/{id}
Accept: application/json
X-API-KEY: {api_key}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 1,
    "ads": [
        {
            "id": "{id}",
            "init": "1711528034170",
            "lastmod": "1721479608368",
            "display": {
                "native": {
                    "link": {
                        "url": "https://example.org/link"
                    },
                    "asset": [
                        {
                            "image": {
                                "url": "https://example.org/image0",
                                "w": 1029,
                                "h": 504
                            }
                        },
                        {
                            "id": 1,
                            "image": {
                                "url": "https://example.org/image1",
                                "w": 984,
                                "h": 552
                            }
                        },
                        {
                            "id": 2,
                            "image": {
                                "url": "https://example.org/image2",
                                "w": 2304,
                                "h": 504
                            }
                        },
                        {
                            "id": 3,
                            "image": {
                                "url": "https://example.org/image3",
                                "w": 3408,
                                "h": 504
                            }
                        }
                    ]
                }
            },
            "audit": {
                "status": 3,
                "init": "1711528034170",
                "lastmod": "1721479608368"
            }
        }
    ]
}
```

<div dir="rtl">

### به روز رسانی بنر

توجه کنید که برای این درخواست باید همه فیلد‌ها ارسال شوند و در غیر این صورت فیلد‌های ارسال نشده پاک می‌شوند.

#### بنر نیتیو

<div dir="ltr">

``` http
PUT {baseUrl}/bidder/{bidderId}/ads/{id} HTTP/1.1
Accept: application/json
X-API-KEY: {api_key}

{
    "display": {
        "native": {
            "link": {
                "url": "https://example.org/link"
            },
            "asset": [
                {
                    "image": {
                        "url": "https://example.org/image0",
                        "w": 1029,
                        "h": 504
                    }
                },
                {
                    "id": 1,
                    "image": {
                        "url": "https://example.org/image1",
                        "w": 984,
                        "h": 552
                    }
                },
                {
                    "id": 2,
                    "image": {
                        "url": "https://example.org/image2",
                        "w": 2304,
                        "h": 504
                    }
                },
                {
                    "id": 3,
                    "image": {
                        "url": "https://example.org/image3",
                        "w": 3408,
                        "h": 504
                    }
                }
            ]
        }
    }
}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 1,
    "ads": [
        {
            "id": "{id}",
            "init": "1711528034170",
            "lastmod": "1721479608368",
            "display": {
                "native": {
                    "link": {
                        "url": "https://example.org/link"
                    },
                    "asset": [
                        {
                            "image": {
                                "url": "https://example.org/image0",
                                "w": 1029,
                                "h": 504
                            }
                        },
                        {
                            "id": 1,
                            "image": {
                                "url": "https://example.org/image1",
                                "w": 984,
                                "h": 552
                            }
                        },
                        {
                            "id": 2,
                            "image": {
                                "url": "https://example.org/image2",
                                "w": 2304,
                                "h": 504
                            }
                        },
                        {
                            "id": 3,
                            "image": {
                                "url": "https://example.org/image3",
                                "w": 3408,
                                "h": 504
                            }
                        }
                    ]
                }
            },
            "audit": {
                "status": 3,
                "init": "1711528034170",
                "lastmod": "1721479608368"
            }
        }
    ]
}
```

<div dir="rtl">

#### بنر بک‌گراندی

توجه کنید که ابعاد بنر ارسالی باید مطابق موارد تعیین شده باشد.

<div dir="ltr">

``` http
PUT {baseUrl}/bidder/{bidderId}/ads/{id} HTTP/1.1
Accept: application/json
X-API-KEY: {api_key}

{
    "display": {
        "w": 1075,
        "h": 1584,
        "banner": {
            "img": "https://example.org/banner-image",
            "link": {
                "url": "https://example.org/url",
                "trkr": [
                    "https://example.org/tracker"
                ]
            }
        },
        "event": [
            {
                "type": 1,
                "url": "https://example.org/loaded"
            },
            {
                "type": 2,
                "url": "https://example.org/impression"
            },
            {
                "type": 2,
                "url": "https://example.org/impression/2?${AUCTION_PRICE}"
            },
            {
                "type": 4,
                "url": "https://example.org/mrc100"
            }
        ]
    }
}

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length:

{
    "count": 1,
    "ads": [
        {
            "id": "id",
            "init": "1724084590878",
            "lastmod": "1724507106939",
            "display": {
                 "w": 1075,
                 "h": 1584,
                "banner": {
                    "img": "https://example.org/banner-image",
                    "link": {
                        "url": "https://example.org/url",
                        "trkr": [
                            "https://example.org/tracker"
                        ],
                        "ext": {
                            "@type": "type.googleapis.com/admanagement.LinkAssetExtension"
                        }
                    },
                    "ext": {
                        "@type": "type.googleapis.com/admanagement.BannerExtension",
                        "img_public": "https://a-banners.divarcdn.com/public-image",
                        "img_public_with_badge": "https://a-banners.divarcdn.com/public-image-with-badge",
                        "type": "BANNER_TYPE_IMAGE"
                    }
                },
                "event": [
                    {
                        "type": 1,
                        "url": "https://example.org/loaded"
                    },
                    {
                        "type": 2,
                        "url": "https://example.org/impression"
                    },
                    {
                        "type": 2,
                        "url": "http://example.org/impression/2?${AUCTION_PRICE}"
                    },
                    {
                        "type": 4,
                        "url": "https://example.org/mrc100"
                    }
                ]
            },
            "audit": {
                "status": 3,
                "init": "1724084590878",
                "lastmod": "1724507106939"
            }
        }
    ]
}
```

<div dir="rtl">

## وضعیت حسابرسی (Audit Status)
هر آگهی یک از [وضعیت‌](https://github.com/InteractiveAdvertisingBureau/AdCOM/blob/main/AdCOM%20v1.0%20FINAL.md#list--audit-status-codes-) های زیر را می‌تواند داشته باشد:
  - Pending Audit
  - Approved
  - Denied
  - Expired

پس از بارگزاری و هر تغیری روی بنر‌ها و لینک، وضعیت حسابرسی به وضعیت در حال انتظار تغییر می‌کند.
تصاویر و لینک‌ها مطابق دستورالعمل انتشار بنر‌های دیوار بررسی می‌شوند. در صورتی که تصاویر و لینک‌ها مطابق دسترالعمل انتشار بنر‌های دیوار باشند، وضعیت حسابرسی به تایید شده تغییر می‌کند. در غیر این صورت، وضعیت حسابرسی به رد شده تغییر می‌کند. پس از گذشت ۱۴ روز از ثبت بنر وضعیت حسابرسی آن به منقضی شده تغییر می‌کند. برای فعال شدن مجدد آن، یک بار باید آگهی بدون تغییر بروز رسانی شود.

## حسابرسی
### بازخورد حسابرسی
مقادیر قابل قبول برای بازخورد حسابرسی به صورت زیر است:
  - نمایش برهنگی
  - محتوای جنسی
  - توهین به ارزش‌های مقدس، اشخاص، گروه‌ها، اقلیت‌ها یا قومیت‌ها
  - استفاده از نمادها، تصاویر و علائم با گرایشات سیاسی
  - انتقال موضوعات نفرت‌انگیز
  - محتوای شامل شوخی‌های جنسی، خون، الکل و مواد مخدر
  - تصاویر آزاردهنده، ناراحت‌کننده، ترسناک یا دلخراش مانند زخم‌ها، خون، حشرات، جراحی و موارد مشابه
  - نمایش شماره تلفن
  - نمایش شناسه شبکه‌های اجتماعی
  - بزرگنمایی‌های آشکار و غیرمنطقی یا ادعاهای بعید
  - متن‌های بزرگ که بیشتر از تصاویر یا نمادها جلب توجه می‌کنند
  - استفاده از زمینه سفید
  - استفاده از زمینه مشکی
  - کیفیت تصویر پایین و تار
  - تصاویر یا متن‌های کشیده شده
  - بج کوچک در گوشه بالا سمت چپ تصویر که شامل متن 'تبلیغ' است
  - استفاده از نام دیوار، به‌ویژه با کلماتی مانند منتخب / همکاری
  - استفاده از پروتکل HTTP
  - استفاده از لینک مستقیم پست‌های دیوار
  - تعداد ریدایرکت‌ها بیش از حد مجاز است
  - لینک معتبر نیست
  - زمان انتظار برای پاسخ از سرور به پایان رسید

### حسابرسی لینک‌های دیوار
برای لینک‌های دیوار موارد زیر باید در نظر گرفته شود. باید توجه شود که اگر از این لینک‌ها به صورت مستقیم استفاده شود، این کار در زمان نمایش  خودکار انجام می‌شود و نیازی به تغییر نیست و در صورت استفاده از لینک‌هایی که به این‌ها ریدایرکت می‌شوند، این تغییرات الزامی هستند.
- به جای لینک‌ پست‌ها (https://divar.ir/v/[TOKEN]) باید از https://a-banners.divar.ir/redirect/v/[TOKEN] استفاده شود.