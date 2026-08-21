```cpp
#include <iostream>
#include <string>
#include <filesystem>
#include <fmt/format.h>

#include "robot_logger/logger_interface.hpp"
#ifdef _WIN32
#include <windows.h>

#include <Eigen/Dense>

#include <variant>

#include "CSDKLoader.h"

#include "IFileService.h"
#include "IRobotService.h"
#include "IMotionService.h"
#include "RobotTypes.h"

using namespace std;
namespace crobot{
constexpr const int WORK_MODE_MANUAL = 0;
constexpr const int WORK_MODE_PLAYING = 1;

class RobotDriver{
public:
	RobotDriver(const RobotDriver&) = delete;
	RobotDriver& operator=(const RobotDriver&) = delete;
	std::filesystem::path exe_dir();
	static RobotDriver& getInstance(){
		static RobotDriver instance;
		return instance;
	}
	static RobotDriver& getInstance(std::shared_ptr<ILogger> logger){
		static RobotDriver instance{logger};
		return instance;
	}
	bool movel(){
	}
	bool movej(){
	}

private:
	RobotDriver(){}

	RobotDriver(std::shared_ptr<ILogger> logger):mLogger(logger){
		mLogger->info("instance");
	}
	
	~RobotDrive(){
	}
	void initialize_sdk();
	
	template<typename T>
	static string getServiceId();

	template<>
	static string getServiceId<Crp::IRobotService>(){
		return ID_ROBOT_SERVICE;
	}
	template<>
	static string getServiceId<Crp::IMOTIONService>(){
		return ID_MOTION_SERVICE;
	}
	template<>
	static string getServiceId<Crp::IMOTIONService>(){
		return ID_FILE_SERVICE;
	}
	
	template<typename T>
	static string getServiceName();

	template<>
	static string getServiceName<Crp::IRobotService>(){
		return "Robot";
	}
	template<>
	static string getServiceName<Crp::IMotionService>(){
		return "Motion";
	}
	template<>
	static string getServiceName<Crp::IFileService>(){
		return "File";
	}
	
	template<typename T>
	std::unique_ptr<T> getService(){
		std::string serviceId = RobotDriver::getServiceId<T>();
		std::string serviceName = RobotDriver::getServiceName<T>();
		auto service = mCRPLoader->getService<T>(serviceId.c_str());
		if(!service){
			throw std::runtime_error(fmt::format("failed to get CRP SDK {} service (ID:{})",serviceName,serviceId));
		}
		return std::unique_ptr<T>(service);
	}

	void servoPowerOn();
	bool cspEnter(){
	}

	string mSDKPath;
	string mCrobotIp;
	string mConnectDev;
	std::string csp_program_;
	std::string pp_program_;

	std::unique_ptr<Crp::CSDKLoader> mCRPLoader;	
	std::unique_ptr<Crp::IRobotService> mRobotSrv;
	std::unique_ptr<Crp::IMotionService> mMotionSrv;
	std::unique_ptr<Crp::IFileService> mFileSrv;
}




```


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzc1NDk3Ml19
-->