# Exception Handle

if biz code no catch the exception ，Zan Framework get to handle the exception.

### ZanHttp Exception Handle Process

1. if exception with redirectUrl is instance of RedirectException，redirect assigned view.
2. if exception is PageNotFoundException，redirect 404 HTML.
3. if exception is InvalidRouteException，redirect 404 HTML.
4. if exception code defined in biz code，response JSON Data or Error HTML by request type.
5. if all above no matched,redirect 500 HTML or 404 HTML.

Exception Handle match order by num 1-5. if matched one ,the other passed.

### Debug Mode

In Debug Mode,its easy to debug by the message of STDOUT after handling above processes.