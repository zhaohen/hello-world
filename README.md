# hello-world
import requests
import time
from win10toast import ToastNotifier

# 初始化通知工具
toaster = ToastNotifier()

def get_gold_price():
    """从公开接口获取国际金价 (XAU/USD)"""
    try:
        # 使用 goldprice.org 的公开数据接口（无需 API Key，适合个人学习）
        url = "https://data-asg.goldprice.org/dbXRates/USD"
        headers = {'User-Agent': 'Mozilla/5.0'}
        
        response = requests.get(url, headers=headers, timeout=10)
        data = response.json()
        
        # 解析数据
        price = data['items'][0]['xauPrice']
        change = data['items'][0]['pcXau'] # 涨跌幅
        
        return price, change
    except Exception as e:
        print(f"获取数据失败: {e}")
        return None, None

def main():
    print("国际金价实时监测小程序已启动...")
    
    while True:
        price, change = get_gold_price()
        
        if price:
            msg = f"当前金价: ${price:.2f}/oz\n今日涨跌: {change:.2f}%"
            print(msg)
            
            # 发送 Windows 弹窗
            toaster.show_toast(
                "实时金价提醒",
                msg,
                duration=10, # 弹窗显示 10 秒
                threaded=True
            )
        
        # 每隔 1 小时检查一次 (3600秒)，可以根据需要修改
        time.sleep(3600)

if __name__ == "__main__":
    main()
