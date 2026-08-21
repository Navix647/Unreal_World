```cpp
#include "driver_interfaces/robot_driver.hpp"

using namespace std;
using namespace crobot;


ifdef _WIN32
std::filesystem::path RobotDriver::exe_dir(){
	std::error_code ec;
	wchar_t path[MAX_PATH];
	
	const uint32_t len = GetModuleFileNameW(NULL,path,MAX_PATH);
	if(len==0){
		return std::filesystem::current_path(ec);
	}
	return std::filesystem::path(std::wstring(path,len)).parent_path();
}
#endif


#ifdef __LINUX__
	constexpr std::size_t buf_size = 4096;
	char path[buf_size];

	ssize_t len = readlink("/proc/self/exe",path,buf_size-1);
	if(len<=0){
		result = std::filesystem::current_path(ec);
	}else{
		path[len] = '\0';
		result = std::filesystem::path(std::string(path)).parent_path();
	}
#endif


void RobotDriver::initialize_sdk(){
	mCRPLoader = std::make_unique<Crp::CSDKLoader>(mSDKPath);
	bool loaderInitRes = mCRPLoader->initialize();
	if(loaderInitRes == false){
		throw std::runtime_error("failed to initialize CRP SDK");
	}
	mRobotSrv = getService<Crp::IRobotService>();
	mMotionSrv = getService<Crp::IMotionService>();
	mFileSrv = getService<Crp::IFileService>();

	bool connResult = mRobotSrv->connect(mCrobotIp.c_str());
	if(connResult == false){
		throw std::runtime_error("failed to initialize CRP SDK");
	}
	
	bool success = mRobotSrv->setWorkMode(WORK_MODE_PLAYING);
	if(success == false){
		throw std::runtime_error("failed to initialize CRP SDK"+mCrobotIp);
	}
	
	if(mRobotSrv->hasError()){
		if(mRobotSrv->hasEmergenceError()){
			throw std::runtime_error("CRP robot has emergency error,clear it manually");
		}
	}
}


void RobotDriver::servoPowerOn()
{
	if(mRobotSrv->isServoOn()){
		return;
	}
	bool success = mRobotSrv->servoPowerOn();
	if(!success){
		throw std::runtime_error("failed to power on servo");
	}
}


```




> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODg0MDAwOTU2XX0=
-->