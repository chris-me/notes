web.xml

```xml
<filter>
  <filter-name>com.company.TomcatFilter</filter-name>
  <filter-class>com.company.tomcatsecurity.TomcatFilter</filter-class>
</filter>

<filter-mapping>
  <filter-name>com.company.tomcatsecurity.TomcatFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

TomcatFilter.java

```java
package de.geis.tomcatsecurity;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

public class TomcatFilter implements Filter {

	@Override
	public void destroy() {
		// TODO Auto-generated method stub		
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		if (request instanceof HttpServletRequest) {
		    HttpServletRequest hrequest = (HttpServletRequest) request;
		    String uri = hrequest.getRequestURI(); 
		    System.out.println(uri);
		    if (uri.contains("foo.bar")) {
		    	throw new ServletException("This URI is protected");
		    }
		}
		chain.doFilter(request, response);
	}

	@Override
	public void init(FilterConfig arg0) throws ServletException {
		// TODO Auto-generated method stub		
	}
}

```
