# Servlet

## Java Servlet 간단 예시

```
package com.example;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(value="/")
public class MyServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html;charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html><body>");
		out.println("Hello, World");
		out.println("</body></html>");
	}
}
```

## Java Servlet 간단 라우팅 예시

```
package com.example;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(value="/")
public class MyServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String uri = reReplacePath(req.getRequestURI());
		System.out.println("uri = " + uri);
		
		PrintWriter out = resp.getWriter();
		resp.setContentType("text/html;charset=utf-8");
		
		if(uri.equals("")) {
			out.println("<html><body>");
			out.println("Hello, World");
			out.println("</body></html>");
		}
		
		if(uri.equals("/ping")) {
			out.println("<html><body>");
			out.println("PONG");
			out.println("</body></html>");
		}
	}
	
	public String reReplacePath(String path) {
		String[] splitPath = path.split("/");
		
		StringBuffer buffer = new StringBuffer();
		for(int i=0; i<splitPath.length; i++) {
			if("".equals(splitPath[i])) {
				continue;
			} else {
				buffer.append("/").append(splitPath[i]);
			}
		}
		
		return buffer.toString();
	}
}
```

## Java Servlet 간단 JSP 라우팅
```
package com.example;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(value="/")
public class MyServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String uri = reReplacePath(req.getRequestURI());
		System.out.println("uri = " + uri);
		
		PrintWriter out = resp.getWriter();
		resp.setContentType("text/html;charset=utf-8");
		
		if(uri.equals("")) {
			RequestDispatcher dispatcher = req.getRequestDispatcher("/index.jsp");
			dispatcher.forward(req, resp);
		}
		
		else if(uri.equals("/ping")) {
			RequestDispatcher dispatcher = req.getRequestDispatcher("/ping.jsp");
			dispatcher.forward(req, resp);
		} else {
			System.out.println("Miss Match");
			RequestDispatcher dispatcher = req.getRequestDispatcher("/404.jsp");
			dispatcher.forward(req, resp);
		}
	}
	
	public String reReplacePath(String path) {
		String[] splitPath = path.split("/");
		
		StringBuffer buffer = new StringBuffer();
		for(int i=0; i<splitPath.length; i++) {
			if("".equals(splitPath[i])) {
				continue;
			} else {
				buffer.append("/").append(splitPath[i]);
			}
		}
		
		return buffer.toString();
	}
}
```