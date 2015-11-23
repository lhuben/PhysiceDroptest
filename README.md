# PhysiceDroptest
//physics物理引擎的尝试

#include "HelloWorldScene.h"

USING_NS_CC;

Scene* HelloWorld::createScene()
{
	//使用物理世界创建场景
	auto scene = Scene::createWithPhysics();

	//显示物理世界调试状态, 显示红色的框, 方便调试
	scene->getPhysicsWorld()->setDebugDrawMask(PhysicsWorld::DEBUGDRAW_ALL);
	//调整重力加速度
	scene->getPhysicsWorld()->setGravity(Vec2(0, -1000));
	auto layer = HelloWorld::create();

	scene->addChild(layer);

	return scene;
}
//创建一个层再执行addEdges,成功调处红框
void HelloWorld::onEnter() {
	Layer::onEnter();
	
	Size visibleSize = Director::getInstance()->getVisibleSize();
	
	auto body = Node::create();
	body->setContentSize(visibleSize);
	body->setPhysicsBody(PhysicsBody::createEdgeBox(visibleSize,PHYSICSBODY_MATERIAL_DEFAULT, 3));
	addChild(body);

}


void HelloWorld::addSprite(cocos2d::Vec2 pos)//
{
	//auto body = Sprite::create("CloseNormal.png");
	//body->setTag(1);
	////设置一个圆形绑定在精灵上面
	//body->setPhysicsBody(PhysicsBody::createCircle(body->getContentSize().width / 2));
	////设置一个正方形绑定在精灵上面
	////body->setPhysicsBody(PhysicsBody::createBox(body->getContentSize()));

	//body->setPosition(pos);
	//addChild(body);

	Size visibleSize = Director::getInstance()->getVisibleSize();

//最新修改 创建正方形掉落test
	auto r = Sprite::create();
	r->setTextureRect(Rect(0, 0, 50, 50));
	r->setPhysicsBody(PhysicsBody::createBox(r->getContentSize()));
	r->setPosition(pos);
	addChild(r);

	switch (rand()%3)
	{
	case 0:
		r->setColor(Color3B(255, 0, 0));
		break;
	case 1:
		r->setColor(Color3B(0, 255, 0));
		break;
	case 2:
		r->setColor(Color3B(0, 0, 255));
		break;
	default:
		break;
	}
}



// on "init" you need to initialize your instance
bool HelloWorld::init()
{
    //////////////////////////////
    // 1. super init first
    if ( !Layer::init() )
    {
        return false;
    }
    
   Size visibleSize = Director::getInstance()->getVisibleSize();
  //  Vec2 origin = Director::getInstance()->getVisibleOrigin();

	addSprite(Vec2(visibleSize.width / 2, visibleSize.height / 2));

	/*
	bool HelloWorld::onTouchBegan(Touch* touch, Event* event)      此处示例init外的方法处理方式与下面的不同  
{  
    Vec2 location = touch->getLocation();  
    addNewSpriteAtPosition(location);  
    return false;  
}     */

	auto listener = EventListenerTouchOneByOne::create();//创建了一个单点触摸事件的监听
	listener->onTouchBegan=[this](Touch* touch, Event* event)//监听开始啦！就是直接onTouchBegan
	{
		Vec2 location = touch->getLocation();
		addSprite(location);
		return false;
	};
	

	Director::getInstance()->getEventDispatcher()->addEventListenerWithSceneGraphPriority(listener, this);
    return true;
}


void HelloWorld::menuCloseCallback(Ref* pSender)
{
    Director::getInstance()->end();

#if (CC_TARGET_PLATFORM == CC_PLATFORM_IOS)
    exit(0);
#endif
}
