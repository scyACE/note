### 使用TS创建react组件

1. 创建tsconfig.json

   ```json
   {
     "compilerOptions": {
       "module": "commonjs",
       "target": "es5",
       "jsx": "react",
       "outDir": "./dist",
       "declaration": true,
       "noEmitOnError": true,
       "lib": ["ES2015"],
       "skipLibCheck": true,//不跳过检查 antd 会报错
     },
     "exclude": [
       "node_modules",
       "dist"
     ]
   }
   ```

2. npm init -y
3. npm i -S react react-dom
4.  npm i -D @types/react @types/react-dom