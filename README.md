# jaop
写着玩的小工具，稍微修改了cglib的源码，实现aop切面的功能

# 代码示例
public class Computer {

    public <E> boolean isEmptyArray(E... array) {
        return null == array
                || array.length == 0;
    }

    public int sum(int... numberArray) {
        if (isEmptyArray(numberArray)) {
            return 0;
        }
        int sum = 0;
        for (int n : numberArray) {
            sum += n;
        }
        return sum;
    }

}

    AOP aop = new AOP.Builder().
            targetClass(Computer.class)
            .targetMethodName("sum")
            .eventListener(new EventListener() {
                @Override
                public void onEvent(Event event) throws Throwable {
                    if(event instanceof BeforeEvent) {
                        final BeforeEvent beforeEvent = (BeforeEvent) event;
                        System.err.println("hello 我是before事件");
    
                        final int[] numberArray = (int[]) beforeEvent.argumentArray[0];
    
                        numberArray[0] = 100;
                        numberArray[1] = 101;
    
                        System.err.println("hello 我修改了参数值");
    
                    }else if(event instanceof ReturnEvent) {
    
                        System.err.println("hello 我是return事件");
                    }
                }
            })
            .build();
    
    Computer computer = (Computer) aop.createIntance();
    
    System.err.println(computer.sum(new int[]{1,1}));

# 输出结果

hello 我是before事件\
hello 我修改了参数值\
hello 我是return事件\
201

