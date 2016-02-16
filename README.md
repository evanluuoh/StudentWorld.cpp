#include "StudentWorld.h"
#include <string>
#include <iostream>
using namespace std;

GameWorld* createStudentWorld(string assetDir)
{
	return new StudentWorld(assetDir);
}

StudentWorld::StudentWorld(std::string assetDir)
	: GameWorld(assetDir)
{
	for (int i = 0; i < 64; i++)
	{
		for (int j = 0; j < 64; j++)
		{
			m_dirt[i][j] = nullptr;
		}
	}
}

StudentWorld::~StudentWorld()
{
	delete m_player;
	for (int i = 0; i < 64; i++)
	{
		for (int j = 0; j < 64; j++)
		{
			delete m_dirt[i][j];
		}
	}
}

int StudentWorld::init()
{
	for (int i = 0; i < 64; i++)
	{
		if (i >= 30 && i <= 33)
			for (int j = 0; j < 4; j++)
			{
				m_dirt[i][j] = new Dirt(IID_DIRT, i, j, GraphObject::right, 0.25, 3, this);
			}
		else
		{
			for (int j = 0; j < 60; j++)
			{
				m_dirt[i][j] = new Dirt(IID_DIRT, i, j, GraphObject::right, 0.25, 3, this);
			}
		}
	}
	m_player = new Frackman(IID_PLAYER, 30, 60, GraphObject::right, 1, 0, this, 10);
	return GWSTATUS_CONTINUE_GAME;
}

int StudentWorld::move()
{
	m_player->doSomething();
	return GWSTATUS_CONTINUE_GAME;
}


void StudentWorld::cleanUp()
{
	delete m_player;
	for (int i = 0; i < 60; i++)
	{
		for (int j = 0; j < 60; j++)
		{
			delete m_dirt[i][j];
		}
	}
}

bool StudentWorld::isDirt(int x, int y)
{
	if (m_dirt[x][y] != nullptr)
	{
		return true;
	}
	return false;
}

void StudentWorld::removeDirt(int x, int y)
{
	delete m_dirt[x][y];
	m_dirt[x][y] = nullptr;
	playSound(SOUND_DIG);
}
// Students:  Add code to this file (if you wish), StudentWorld.h, Actor.h and Actor.cpp
