# Extensions in BApp Store for automation scanning (Required Burp Suite Professional)

- Comming Soon

# Bambda for HTTP History filter

## How can use:
1. Open Burp Suite
2. Proxy -> HTTP History -> Filter Settings -> HTTP History filter -> Script Mode -> Paste Bamba Filter

Sample:

<img width="948" height="419" alt="image" src="https://github.com/user-attachments/assets/d19ab424-78f4-4829-bb39-dd313c421e0e" />

---

- Script Mode 1 not using feature `show in scopes` not filter `MIME type`:

```bambda
String url = requestResponse.url().toLowerCase();
String urlWithoutParams = url;
int paramIndex = url.indexOf("?");
if (paramIndex != -1) {
    urlWithoutParams = url.substring(0, paramIndex);
}

// endpoints
if (url.contains("/v1/metrics") ||
    url.contains("/analytics") ||
    url.contains("/tracking") ||
    url.contains("/collect") ||
    url.contains("/pixel.gif") ||
    url.contains("/api/telemetry") ||
    url.contains("/api/log") ||
    url.contains("/__dux") ||
    url.contains("/telemetry") ||
    url.contains("/unstable/produce_batch") ||
    url.contains("/v1/produce") ||
    url.contains("/v1/traces") ||
    url.contains("/.well-known/dux") ||
    url.contains("/logging") ||
    url.contains("/events") ||
    url.contains("/v1/r/d/b/p1") ||
    url.contains("/v1/r/d/b/p2") ||
    url.contains("/v1/logs")) {
    return false;
}

// domains
if (url.contains("px.ads.linkedin.com") ||
    url.contains("googletagmanager.com") ||
    url.contains("facebook.com/tr") ||

    url.contains("www.facebook.com") ||
    url.contains("connect.facebook.net") ||
    url.contains("www.recaptcha.net") ||
    url.contains("t.paypal.com/ts") ||
    url.contains("error-analytics-sessions-production.shopifysvc.com") ||
    url.contains("doubleclick.net") ||
    url.contains("analytics.google.com") ||
    url.contains("hotjar.com") ||
    url.contains("mixpanel.com") ||
    url.contains("segment.com") ||
    url.contains("clarity.ms") ||
    url.contains("adservice.google.com") ||
    url.contains("www.google.com.vn")) {
    return false;
}

// extensions file
if (urlWithoutParams.endsWith(".woff") ||
    urlWithoutParams.endsWith(".woff2") ||
    urlWithoutParams.endsWith(".ttf") ||
    urlWithoutParams.endsWith(".eot") ||
    urlWithoutParams.endsWith(".ico") ||
    urlWithoutParams.endsWith(".png") ||
    urlWithoutParams.endsWith(".jpg") ||
    urlWithoutParams.endsWith(".jpeg") ||
    urlWithoutParams.endsWith(".gif") ||
    urlWithoutParams.endsWith(".svg") ||
    urlWithoutParams.endsWith(".webp") ||
    urlWithoutParams.endsWith(".css") ||
    urlWithoutParams.endsWith(".js") ||
    urlWithoutParams.endsWith(".css.map") ||
    urlWithoutParams.endsWith(".js.map")) {
    return false;
}

return true;
```

---

- Script Mode 2 using `show in scopes` and have filter `MIME type`:

```bambda
String path = requestResponse.request().path();
String method = requestResponse.request().method();

// Exclude methods
String[] excludeMethods = {"OPTIONS", "HEAD"};
boolean isMethodExcluded = false;
for (String excludeMethod : excludeMethods) {
    if (method.equals(excludeMethod)) {
        isMethodExcluded = true;
        break;
    }
}

// Exclude paths
String[] excludePaths = {
    "/shell/navigation/trackEvent",
    "/api/analytics",
    "/shell/navigation/tracking/track",
    "/authentication-proxy/proxy/otel/v1/traces",
    "/portal/api/tracking",
    "/tracking"
};
boolean isPathExcluded = false;
for (String excludePath : excludePaths) {
    if (path.contains(excludePath)) {
        isPathExcluded = true;
        break;
    }
}

// Exclude file extensions (CUSTOM HERE)
String[] excludeExtensions = {
    ".js", ".css", ".jpg", ".jpeg", ".png", ".gif", 
    ".svg", ".ico", ".woff", ".woff2", ".ttf", ".eot",
    ".map", ".webp", ".mp4", ".mp3", ".pdf", ".zip"
};
boolean isExtensionExcluded = false;
String lowerPath = path.toLowerCase();
for (String extension : excludeExtensions) {
    if (lowerPath.endsWith(extension)) {
        isExtensionExcluded = true;
        break;
    }
}

return requestResponse.request().isInScope() &&
       !isMethodExcluded &&
       !isPathExcluded &&
       !isExtensionExcluded;
```

---

## How can I reuse it for future occasions?

### Save Bambda filter
1. Open Burp Suite
2. Proxy -> HTTP History -> Filter Settings -> HTTP History filter -> Script Mode -> Paste Bamba Filter -> Save to librabry... -> save -> add name -> save

- Sample
<img width="961" height="458" alt="image" src="https://github.com/user-attachments/assets/737f0aab-9fed-45e3-80f6-9a553436c61d" />

---

### Bambda filter Load
1. Proxy -> HTTP History -> Filter Settings -> HTTP History filter -> Script Mode -> Load -> choise name bambda filter

- Sample:
<img width="947" height="417" alt="image" src="https://github.com/user-attachments/assets/1564775c-f85d-4b53-b94b-495a5380d7a5" />
