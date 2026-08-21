```cpp
#pragma once
#include <fmt/format.h>
#include <string>
#include <iostream>

enum class LogLevel {
    Trace,
    Debug,
    Info,
    Warn,
    Error,
    Fatal
};

class ILogger {
public:
    virtual ~ILogger() = default;

    // 公共模板方法（每个级别一个，但都转发给 log_impl）
    template<typename... Args>
    void trace(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Trace, fmt, std::forward<Args>(args)...);
    }
    template<typename... Args>
    void debug(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Debug, fmt, std::forward<Args>(args)...);
    }
    template<typename... Args>
    void info(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Info, fmt, std::forward<Args>(args)...);
    }
    template<typename... Args>
    void warn(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Warn, fmt, std::forward<Args>(args)...);
    }
    template<typename... Args>
    void error(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Error, fmt, std::forward<Args>(args)...);
    }
    template<typename... Args>
    void fatal(fmt::format_string<Args...> fmt, Args&&... args) {
        log(LogLevel::Fatal, fmt, std::forward<Args>(args)...);
    }

protected:
    // 子类必须实现这个单一虚函数
    virtual void log_impl(LogLevel level, const std::string& msg) = 0;

private:
    // 内部统一格式化并捕获异常
    template<typename... Args>
    void log(LogLevel level, fmt::format_string<Args...> fmt, Args&&... args) {
        std::string msg;
        try {
            msg = fmt::format(fmt, std::forward<Args>(args)...);
        } catch (const std::exception& e) {
            std::cerr << "[ILogger] Format error: " << e.what() << std::endl;
            msg = "[ILogger format error]";
        } catch (...) {
            std::cerr << "[ILogger] Unknown format error" << std::endl;
            msg = "[ILogger unknown format error]";
        }
        log_impl(level, msg);
    }
};

class SpdlogAdapter : public ILogger {
protected:
    void log_impl(LogLevel level, const std::string& msg) override {
        if (!logger_) return;
        switch (level) {
            case LogLevel::Trace:   logger_->trace(msg); break;
            case LogLevel::Debug:   logger_->debug(msg); break;
            case LogLevel::Info:    logger_->info(msg);  break;
            case LogLevel::Warn:    logger_->warn(msg);  break;
            case LogLevel::Error:   logger_->error(msg); break;
            case LogLevel::Fatal:   logger_->critical(msg); break;
        }
    }
};

class RclcppLoggerAdapter : public ILogger {
public:
 explicit RclcppLoggerAdapter(const rclcpp::Logger& logger) : logger_(logger) {}
protected:
 void log_impl(LogLevel level, const std::string& msg) override {
 switch (level) {
 case LogLevel::Trace:   RCLCPP_DEBUG(logger_, "%s", msg.c_str()); break;
 case LogLevel::Debug:   RCLCPP_DEBUG(logger_, "%s", msg.c_str()); break;
 case LogLevel::Info:    RCLCPP_INFO(logger_,  "%s", msg.c_str()); break;
 case LogLevel::Warn:    RCLCPP_WARN(logger_,  "%s", msg.c_str()); break;
 case LogLevel::Error:   RCLCPP_ERROR(logger_, "%s", msg.c_str()); break;
 case LogLevel::Fatal:   RCLCPP_FATAL(logger_, "%s", msg.c_str()); break;
 default: break;
 }
 }
private:
 rclcpp::Logger logger_;
};

```



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMjAwMjYyNjBdfQ==
-->