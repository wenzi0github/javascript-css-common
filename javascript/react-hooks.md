# react 中的自定义 hooks

### 获取浏览器窗口的宽度和高度

```typescript
const useWinResize = () => {
    const [size, setSize] = useState({
        width: document.documentElement.clientWidth,
        height: document.documentElement.clientHeight
    });
    const resize = useCallback(() => {
        setSize({
            width: document.documentElement.clientWidth,
            height: document.documentElement.clientHeight
        });
    }, []);
    useEffect(() => {
        window.addEventListener('resize', resize);
        return () => window.removeEventListener('resize', resize);
    }, []);
    return size;
};
```

使用方式：

```typescript
const Home = () => {
    const { width, height } = useWinResize();

    return (
        <div>
            <p>width: {width}</p>
            <p>height: {height}</p>
        </div>
    );
};
```

### 自定义的定时器

可以通过调整`delay`的值，来启动或停止定时器

```typescript
const useInterval = (callback, delay) => {
    const saveCallback = useRef();

    useEffect(() => {
        // 每次渲染后，保存新的回调到我们的 ref 里
        saveCallback.current = callback;
    });

    useEffect(() => {
        function tick() {
            saveCallback.current();
        }
        if (delay !== null) {
            let id = setInterval(tick, delay);
            return () => clearInterval(id);
        }
    }, [delay]);
};
```

使用方式：

```typescript
const [count, setCount] = useState(0);
const [diff, setDiff] = useState(500);

useInterval(() => {
    setCount(count + 1);
}, diff);
```
