# spring 中的统一异常处理方式

在SSM 项目中，Controller层为处于请求处理的最顶层，再往上就是框架代码的。
因此，肯定需要在Controller捕获所有异常，并且做适当处理，返回给前端一个友好的错误码。

但是Controller多了，就会发现Controller中有许多重复冗余的代码，那么可以将异常处理抽取出来，使Controller专注于逻辑业务的处理，使得异常处理有个统一的控制中心点。

### 全局异常处理

HandlerExceptionResolver接口

	public interface HandlerExceptionResolver {
	    ModelAndView resolveException(HttpServletRequest var1, HttpServletResponse var2, Object var3, Exception var4);
	}


使用全局处理器只需要两部：

1. 实现HandlerExceptionResolver接口。
将实现类作为Spring Bean，这样Spring就能扫描到它并作为全局异常处理器加载

2. 在resolveException中实现异常处理逻辑。
从参数上，可以看到，不仅能够拿到发生异常的函数和异常对象，还能够拿到HttpServletResponse对象，从而控制本次请求返回给前端的行为。


函数还可以返回一个ModelAndView对象，表示渲染一个视图，比方说错误页面。
不过，在前后端分离为主流架构的今天，这个很少用了。如果函数返回的视图为空，则表示不需要视图。


### Controller局部异常处理


 @ExceptionHandler注解
这种异常处理只局部于某个Controller内。，它还能够对异常类型进行细粒度的控制，通过注解可以有选择的指定异常处理方法应用的异常类型：

		@ExceptionHandler({BusinessException.class, DataBaseError.class })

###  ControllerAdvice

义一个存放异常处理函数的类，并使用@ControllerAdvice修饰.

	@ControllerAdvice(assignableTypes = {GlobalExceptionHandlerMixin.class})
	public class ExceptionAdvice {
	    @ExceptionHandler(ErrorCodeWrapperException.class)
	    @ResponseBody
	    public ResponseDTO<?> exceptionHandler(ErrorCodeWrapperException e) {
	        if ((errCodeException.getErrorCode().equals(ErrorCode.SYSTEM_ERROR))) {
	            log.error(e);
	        }
	        return ResponseDTO.ofErroCodeWrapperException(errCodeException);
	    }
	}


@ExceptionHanlder修饰的方法的写法和Controller内的异常处理函数写法是一样的。


@ControllerAdvice(assignableTypes = {GlobalExceptionHandlerMixin.class}),它用来限定这些异常处理函数起作用的Controller的范围。如果不写，则默认对所有Controller有效。

这是ControllerAdvice进行统一异常处理的优点，它能够细粒度的控制该异常处理器针对哪些Controller有效


例子，只针对实现了GlobalExceptionHandlerMixin接口的类有效:

	@Controller
	@Slf4j
	@RequestMapping("/api/demo")
	public class DemoController implements GlobalExceptionHandlerMixin {
	}

ControllerAdvice支持的限定范围:

1. 按注解：@ControllerAdvice(annotations = RestController.class)
2. 按包名：@ControllerAdvice("org.example.controllers")
3. 按类型：@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})



理论上，任何能够给Controller加切面的机制都能变相的进行统一异常处理。比如：

1. 在拦截器内捕获Controller的异常，做统一异常处理。
2. 使用Spring的AOP机制，做统一异常处理。